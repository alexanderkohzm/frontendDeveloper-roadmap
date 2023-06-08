# What are Client and Server Components?

[https://www.freecodecamp.org/news/react-server-components-for-beginners/](https://www.freecodecamp.org/news/react-server-components-for-beginners/) 

React server components blend server-side rendering with the interactivity of client-side JavaScript. 

### Problems…

Imagine we have this component: 

```tsx
const app = () => {

 return (
		<WrapperComponent>
			<ComponentA/>
			<ComponentB/>
    </WrapperComponent>
  )
}
```

And imagine all components have to get their own data. 

```tsx
const WrapperComponent = ({children}) => {

	const [data, setData] = useState()

  useEffect(() => {
		
		const data = 'CALL API'
    setData(data)
  })

	return (
    {data && children}
  )
}

// Imagine the same for Component B
const ComponentA = () => {
  const [componentAData, setComponentAData] = useState({});
  
  useEffect(() => {
    getComponentAData().then(res => {
      setComponentAData(res.data);
    });
  }, []);
  
  return (
  	<>
      <h1>{componentAData.name}</h1>
    </>
  )
}
```

The issue with this occurs when the different components require different amount of times to fetch their data. For example, if WrapperComponent requires 1 second, then Component A and Component B will not load until WrapperComponent has successfully fetched the data. Following that, if Component B takes 1 second but Component A takes 3 seconds, component B will suddenly be pushed down and Component A will appear when it loads. 

This is called the ******************************Waterfall Issue******************************. Sequential fetch requests are only initiated after previous fetch requests have been resolved or completed. 

One solution is to fetch ALL the data in the top most component (App) and then pass it down. The issue with this is that the API response becomes very coupled with the components - for example, if we remove Component A then we need to remove Component A data from the API response. 

### Solution

One solution is to use **********************************Server Components**********************************. These components are rendered in the server. Moving them there allows for data fetching to be much quicker - you have direct access to the server and you can query from your DB directly from your component. 

The catch is that you ********************************cannot use hooks******************************** (useState, useEffect, web API, event handlers) like a normal react component. You can use async/await though.

**********************************Client Components********************************** are the regular components you’ve been writing up to this point. With the latest updates to react, now you need to specify that these components are client components with the `use client` phrase. 

### Why would we want Server Components?

Server components allow devs to render **************************************static information************************************** on the website. You can also use 3rd party packages and not have to worry about bundle sizing.