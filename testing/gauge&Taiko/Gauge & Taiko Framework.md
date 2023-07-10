# Gauge & Taiko Framework

**Context:**

I had to learn Gauge & Taiko framework in 2 days for an online assessment for a company.

### What is Gauge and Taiko?

Gauge is a free open-source framework for automated testing. There are three main benefits that Gauge brings if a developer chooses to adopt it.

**Gauge**

**First**, it promotes **separation of specifications and implementation.** Rather than having a test suite like Jest where the tests (e.g. `Can type in Name field`) and the implementation exist in one file, Gauge’s framework separates the test and its implementation. Tests are listed in a Markdown file called a **Specification** while the implementation to execute the tests are listed in a separate file in a **Test** folder. Note that Gauge tries to match Specifications and Tests together so you should name both files the same. For example, if you want to test the Registration page, you should name the specification file `registration.spec` and the file in the test folder `registration.js`.

**Second**, it promotes **reusability** of steps or code snippets. You can create generic steps that encapsulate common actions (e.g. type into `XYZ` field) and reuse them across different test scenarios. This reduces code duplication and improves maintainability.

**Third**, it promotes **collaboration across different stakeholders.** Gauge’s **Specification files** in a markdown format promotes collaboration as it provides a common language that can be easily understood by testers, developers, and stakeholders. The markdown file which describes scenarios and test steps **creates an abstraction** that allows for people who may not have the technical grounding to **understand the steps and contribute their insights and opinion on what should be be tested**.

**Taiko**

Taiko is essentially a wrapper or API around an automation tool (i.e. Puppeteer, Selenium). Through abstraction, it provides a layer of commands that is easily readable and usable by people to direct the automation tool. For example, commanding the automation tool to enter values into a field in a form. It also provides other powerful utility tools, for example selecting based on **proximity to elements nearby**.

Taiko also includes the option to use more powerful and direct commands, for example allowing developers who are familiar with `XPath` to select an element by `XPath` rather than with the proximity tools or through the abstracted layer.

### Could you explain how reusability works in Gauge and Taiko?

There are a few ways you could promote reusability in Gauge and Taiko - it depends on your preferences and which layer you would like to abstract out the reuse of code.

Since Gauge is meant to be human-readable and across different teams, I suggest that it’s better to keep the abstraction and reusing of code in the test layer. This ensures that the **Specification (Markdown) layer** just handles the concern of making things precise and easy to read for all team members (Product Manager, QA, Software Engineer) while the QA/Software Engineers can handle the abstractions/reusability in the **Test Layer**

Another potential issue you might run into if you reuse in the **Markdown layer** is that your steps become **heavily coupled**. This is quite dangerous because certain fields might require different methods etc

**Easily Readable - Specification (Markdown) layer**

```markdown
### Fields

#### Name

- Open Register Page
- Type into Name field

#### Display Name

- Type into Display Name field

#### Emails

- Type into Email field
- Email must be a valid email
```

**Test Layer**

```jsx
step("Type into Name field", async function () {
  const value = "Test Name";
  await write(value, "Name");
  const nameField = textBox("Name");
  const actualValue = await nameField.value();

  assert.strictEqual(
    actualValue,
    value,
    "Name field value does not match input value"
  );
});

step("Type into Display Name field", async function () {
  const value = "Test Display Name";
  await write(value, "Display Name");
  const displayNameField = textBox("Display Name");
  const actualValue = await displayNameField.value();

  assert.strictEqual(
    actualValue,
    value,
    "Display Name field value does not match input value"
  );
});

//... and so on for different fields
```

**Not so easy to read and more coupled - Specification (Markdown) layer**

```markdown
### Fields

#### Name

- Open Register Page
- Type into field "Name" "TestName"

#### Display Name

- Type into field "Display Name" "Test Display Name"

#### Emails

- Type into field "Email" "testEmail@email.com"
```

**Test Layer**

```jsx
step("Type into field <field> <value>", async function (field, value) {
  await write(value, field);
  const field = textBox(field);
  const actualValue = await field.value();

  assert.strictEqual(
    actualValue,
    value,
    `${field} field value does not match input value`
  );
});
```

**Reusable Methods**

```jsx
const defaultInputMethod = async (field, value) => {
  await click(field);
  await write(value);
};

const inputWithId = async (field, value, id) => {
  const targetField = textBox({ id }, below(field));
  await write(value, targetField);
};

const inputDropdown = async (field, value, id) => {
  const targetField = textBox({ id }, below(field));
  await write(value, targetField);
  await press("Enter");
};
```

**Example Integration Test**

```jsx
### Able to register an account
* Register by filling up the form with table of values

| Field             | Value         | InputMethod  | Id                |
|-------------------|---------------|--------------|-------------------|
| Name              | Test Name     | Default      |                   |
| Display Name      | Testing123    | Default      |                   |
| Email             | test@email.com| Default      |                   |
| Password          | nJhYt#134     | Default      |                   |
| Confirm Password  | nJhYt#134     | ById         | password-confirm  |
| Country           | Singapore     | Dropdown     |   country         |
| Country Code      | +65           | Dropdown     |   input-124       |
| Contact Number    | 88888888      | ById         |   input-129       |
```

```jsx
step(
  "Register by filling up the form with table of values <table>",
  async (table) => {
    for (const row of table.rows) {
      const field = row.cells[0];
      const value = row.cells[1];
      const inputMethod = row.cells[2];
      const id = row.cells[3];

      // !IMPORTANT!
      // Email, ConfirmPassword, Country, and PhoneNumber fields cannot be written into directly
      // and returns an error. We need to use different methods for different fields
      // Example Error below
      // Error: TextBox with label Email is not writable

      // This switch statement is not optimal
      // There must be a more reliable way of writing into fields
      // But that'll only be certain after more experience - this is my first time
      // using Gauge and Taiko

      switch (inputMethod) {
        case "Default":
          // need to randomise email to make sure it's unique
          if (field === "Email" || field === "Display Name") {
            const randomString = generateRandomString(6);
            const uniqueEmail = `${randomString}${value}`;
            await defaultInputMethod(field, uniqueEmail);
          } else {
            await defaultInputMethod(field, value);
          }
          break;
        case "ById":
          await inputWithId(field, value, id);
          break;
        case "Dropdown":
          await inputDropdown(field, value, id);
          break;
        default:
          console.log(`Error, no inputMethod passed in`);
      }
    }

    // agree to T&C
    await click("I agree to");

    // click on register
    waitFor(1000);
    await click(
      button(
        "Register",
        below("I agree to terms and conditions and privacy policy")
      )
    );

    // wait for request to create account
    waitFor(5000);

    // Registered Successfully modal
    const successModalHeader = text("Registered Successfully!");
    const successModalHeaderExists = await successModalHeader.exists();
    const successMessage = text(`We've sent an email`);
    const successMessageExists = await successMessage.exists();

    assert(
      successModalHeaderExists && successMessageExists,
      "Registration Integration test failed, unable to find success modal header or success message"
    );
  }
);
```
