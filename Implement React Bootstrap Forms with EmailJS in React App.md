This article will glide you through the process of building a [React-Bootstrap](https://react-bootstrap.github.io) form component with a working mailing system using [EmailJS](https://www.emailjs.com).


##PREREQUISITES:

> npm or yarn
> Any Editor
> EmailJS Account


##INSTALLATION:



We will be using create-react-app for this particular project. Open Terminal / cmd in the directory you want your project to reside on and enter the command:

```
npx create-react-app <app-name>
```

In this example our app name is 'form'.


Enter into the directory:
```
cd form
```
In the folder you can see we have a framework ready and working, this tree directory consists of a folder named as 'node_modules' which essentially contains all the components provided by create-react-app.

We are going to need the react-bootstrap module and we can install it using the command:

```
npm install react-bootstrap bootstrap
```

Now we have the modules ready you can test run the app by using the command:

```
npm start
```

After running this command, the default react app will be hosted at your localhost.

It would look like this:

![React App](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/buu36535k13mmwhncalo.png)

##Getting Started

In the `src` folder you can see a file named App.js. This is where your application components resides in.

In this `src` folder create a new file named Form.js and add import the following components into it:

```javascript
import React from 'react';
import { Form, Col, Button } from 'react-bootstrap';
```

Now, lets create a Form compnent with our required fields, In this example the fields will be:

    1. Name
    2. Email
    3. Mobile No.
    4. Query


In the `src/Form.js` add this:

```javascript
export const FormPage = (props) => {
    return (

        <Form>
            <Form.Group as={Col} controlId="formGridName">
                <Form.Label>Name*</Form.Label>
                <Form.Control name="name" type="name" placeholder="Name" />
            </Form.Group>

            <Form.Group as={Col} controlId="formGridEmail">
                <Form.Label>Email*</Form.Label>
                <Form.Control name="email" type="email" placeholder="Enter email"
                />
            </Form.Group>
            <Form.Group as={Col} controlId="formGridMobile">
                <Form.Label>Mobile no.*</Form.Label>
                <Form.Control name="mobile" placeholder="" />
            </Form.Group>
            <Form.Group as={Col} id="formGridQuery">
                <Form.Label>Query*</Form.Label>
                <Form.Control name="query" as="textarea" rows={3} />
            </Form.Group>

            <Button variant="primary" type="submit">
                Submit
                </Button>
        </Form >

    )
}
```
Now let us break down this code:

```javascript
<Form.Group as={Col} controlId="formGridName">
    <Form.Label>Name*</Form.Label>
    <Form.Control name="name" type="name" placeholder="Name" />
</Form.Group>
```

First, We create a Form using the `<Form>` tag we imported from react-bootstrap. We utilise the `<Form.Group>` tag for our different entries and `<Form.Label>` to Label those entries.

The `<Form.Control>` is used for the inline box which recives the input.

The `as={Col}` is used to form grid system which is provided by react-bootstrap, All `<Form.Group>` will be in a column.

We can also adjust the amount of rows an inline box must have by adding `rows = {n}` in `<Form.Control>` tag where 'n' is the number of rows.

```javascript
<Form.Control name="query" as="textarea" rows={3} />
```

We then add a Button to submit all the data:

```javascript
<Button variant="primary" type="submit">
    Submit
</Button>
```
Now, that our FormPage is ready we only have to import this component to our App.js.


In `src/App.js` replace it all with this:
```javascript
import React from 'react'
import { FormPage } from './Form';
import 'bootstrap/dist/css/bootstrap.min.css';

function App() {
  return (
    <>
      <React.Fragment>
        <FormPage></FormPage>
      </React.Fragment>
    </>
  )
}
export default App;
```

We simply import the component : `import { FormPage } from './Form';`

And here we also import the styling for the bootstrap form:
`import 'bootstrap/dist/css/bootstrap.min.css';`

When you run `npm start` the result will look like this:
![Form](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pj4e87ceh3nyeig5wz4p.png)

####Congratulations, We are 50% completed with the process. Now the last thing left for us is to make an EmailJS account and connect it to our form!!!


##Setting up EmailJS Account

Create EmailJS account [here](https://dashboard.emailjs.com/sign-up)

After signing up, your dashboard will look like this:
![Dashboard](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/f745zlamz6gfgb44iki9.png)

Now you can add a service and connect it to your email.

![Service](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/45kyfjciyfz7gexhu3qj.png)

After you have connected your email, your service is ready!

![Servie done](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fvoiovbb78trq3vrbei5.png)

Take note of the serviceID we will need that later.

Go to `Integration -> Browser` to get script needed to use our service into our app.

![Browser](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sxblnn007uxbwxnqkm01.png)


Copy the provided script and paste it in the `<head>` tag of the `public/index.html` file.

Next step is to make a template, Go to Email Templates and click on create new template.
![Template](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k6a7y1uyhev9rvfk3reu.png)

You can change the template values here represented as `{{ value }}`, these will essentially match the data values in our code as well.

This is the template been used in this example:
![Template](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nzwfmonsvjri8tfzvp9n.png)

![Template Done](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/f2r0547padt6cufu7576.png)

Take note of the template Id, we will be needing that.


##Implementing the Service

Back to our `src/Form.js`, The only thing left for us is to store the data provided through our form. For this will be declaring a const that stores these values for us.

There are several methods to do this but one of the simpler one is by using `Object.freeze`.

Hence we introduce `initialFormData` as a storing constant:
```javascript
const initialFormData = Object.freeze({
    username: "",
    email: "",
    mobile: "",
    query: ""
  });
```

As you can observe all these values matches our Form components. 

Add this under FormPage component:
```javascript
const [formData, updateFormData] = React.useState(initialFormData);
```
This will hook the data without in need of any classes.


Now, Inside our FormPage component we add handleChange constant to grab values from the form groups.

```javascript
const handleChange = (e) => {
        updateFormData({
          ...formData,

          [e.target.name]: e.target.value.trim()
        });
      };
```

The above code will actively take input values as well as trim trailing white spaces.

Now we change our form groups to utilize this:
```javascript

<Form>
    <Form.Group as={Col} controlId="formGridName">
        <Form.Label>Name*</Form.Label>
        <Form.Control onChange= {handleChange} name="name" type="name" placeholder="Name" />
    </Form.Group>

    <Form.Group as={Col} controlId="formGridEmail">
        <Form.Label>Email*</Form.Label>
        <Form.Control onChange= {handleChange} name="email" type="email" placeholder="Enter email"
        />
    </Form.Group>
    <Form.Group as={Col} controlId="formGridMobile">
        <Form.Label>Mobile no.*</Form.Label>
        <Form.Control onChange= {handleChange} name="mobile" placeholder="" />
    </Form.Group>
    <Form.Group as={Col} id="formGridQuery">
        <Form.Label>Query*</Form.Label>
        <Form.Control onChange= {handleChange} name="query" as="textarea" rows={3} />
    </Form.Group>

    <Button variant="primary" type="submit">
        Submit
        </Button>
</Form >

```

In the above snippet we simply added `onChange = {handleChange}` to each Form groups's Control attribute.

Now we need to submit these stored values, we can simply implement this by introducing another `const` inside FormPage:

```javascript
const handleSubmit = (e) => {
    e.preventDefault()
    alert(`Thank you for your message. Your query has been forwarded.`);
    const templateId = 'template_4oug267';
    const serviceID = "service_kqkanza";
    sendFeedback(serviceID, templateId, { from_name: formData.name, mobile: formData.mobile, message_html: formData.query, email: formData.email })

    console.log(formData);
    };
```

We will be adding another variable sendFeedback that we will send all the data with the serviceID and templateID of our EmailJS service. The above snippet does just that and also makes a prompt alerting the user that their email is being sent.
`e.preventDefault()` overrides the default submit method of bootstrap.

We will now make the variable to process `    sendFeedback(serviceID, templateId, { from_name: formData.name, mobile: formData.mobile, message_html: formData.query, email: formData.email })`:

```javascript
    const sendFeedback = (serviceID, templateId, variables) => {
        window.emailjs.send(
            serviceID, templateId,
            variables
        ).then(res => {
            console.log('Email successfully sent!')
        })
            .catch(err => console.error('There has been an Error.', err))
    }
```
Ener the serviceID and templateID provided by your service which you noted earlier on.

We bind this function to the Submit button:

```javascript
<Button onClick={handleSubmit} variant="primary" type="submit">
    Submit
</Button>
```

Your final `src/Form.js` will look like this:
```javascript
import React from 'react';
import { Form, Col, Button } from 'react-bootstrap';

const initialFormData = Object.freeze({
    username: "",
    email: "",
    mobile: "",
    query: ""
  });


export const FormPage = (props) => {
    const [formData, updateFormData] = React.useState(initialFormData);

    const sendFeedback = (serviceID, templateId, variables) => {
        window.emailjs.send(
            serviceID, templateId,
            variables
        ).then(res => {
            console.log('Email successfully sent!')
        })
            .catch(err => console.error('There has been an Error.', err))
    }

    const handleChange = (e) => {
        updateFormData({
          ...formData,

          [e.target.name]: e.target.value.trim()
        });
      };

    const handleSubmit = (e) => {
        e.preventDefault()
        alert(`Thank you for your message. Your query has been forwarded.`);
        const templateId = 'template_4oug267';
        const serviceID = "service_kqkanza";
        sendFeedback(serviceID, templateId, { from_name: formData.name, mobile: formData.mobile, message_html: formData.query, email: formData.email })

        console.log(formData);
      };

    return (

        <Form>
            <Form.Group as={Col} controlId="formGridName">
                <Form.Label>Name*</Form.Label>
                <Form.Control onChange= {handleChange} name="name" type="name" placeholder="Name" />
            </Form.Group>

            <Form.Group as={Col} controlId="formGridEmail">
                <Form.Label>Email*</Form.Label>
                <Form.Control onChange= {handleChange} name="email" type="email" placeholder="Enter email"
                />
            </Form.Group>
            <Form.Group as={Col} controlId="formGridMobile">
                <Form.Label>Mobile no.*</Form.Label>
                <Form.Control onChange= {handleChange} name="mobile" placeholder="" />
            </Form.Group>
            <Form.Group as={Col} id="formGridQuery">
                <Form.Label>Query*</Form.Label>
                <Form.Control onChange= {handleChange} name="query" as="textarea" rows={3} />
            </Form.Group>

            <Button onClick={handleSubmit} variant="primary" type="submit">
                Submit
                </Button>
        </Form >

    )
}

```

##Voila!!

Your App is done. Here is snapshots of how it works:
![entries](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/azuyga7fzqglg75tkwft.png)
![sent](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fisxh3p91qsckziy7lc1.png)
![recieved](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g44618q1zoc361ten4tx.png)

You can find the github repo [here](https://github.com/rajUwU/form)

There you have it, Thanks for reading.





