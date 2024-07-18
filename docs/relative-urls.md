## **Base URL setup for the relative URL’s**

While using only blade files it's quite easy to add a base URL as a WordPress site URL by using a method ```get_site_url()```. But when we need to use react.js for frontend we can't directly use this method inside the react components as we are restricted to add the php code inside javascript files. So, here we have a workaround for this by using the following ways:

- Pass site url as a HTML attribute from the react component containing HTML Element(div, section, etc.). We can use the main HTML element from the template file(app.blade.php) to set an atrribute.  
``` <main id="main" class="main" site-url="{{ get_site_url() }}"> ```

- Then we need to read the attribute value inside js file using ```getAttribute()``` method and store it inside a variable. We can create the variable inside App.js file so that it is accessible for all components.  
``` const site_url = document.getElementById('main').getAttribute('site-url'); ```

- Now, we can pass the variable to multiple components as a props and can use it to set a base URL for relative URL's.  
``` <MainMenu siteUrl={siteUrl} /> ```

Moreover, Instead of passing it as prop to each component, Alternatively we can create one method in App.js and then we can import that method inside the component something like this:

>App.js

```
const baseURL = (urlString) => {
  let site_url = null;
  if (urlString.indexOf('http://') === 0 || urlString.indexOf('https://') === 0 
  || urlString.indexOf('www') === 0 || urlString.indexOf('mailto') === 0 || 
  urlString.indexOf('tel') === 0) {
    site_url = '';
  } else {
    site_url = document.getElementById('main').getAttribute('site-url');
  }

  return site_url+urlString;
}
```

> YourComponent.js

```
import baseUri from '@scripts/app';
const YourComponent = () => {
    return(
        <>
            <a href={baseUri('/example')}>Your Component</a>
        </>
    )
}
export default YourComponent
```