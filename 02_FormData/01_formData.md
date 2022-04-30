You Can Use [this](https://github.com/JoinCODED/Demo-Express-M4-FE) as a starting point for front end.

In `App.js` you will find a signup form.

1. In this form, change the image's input field type from text to file and remove the value. The value of an input tag of type file is read-only, so that means it has to be an Uncontrolled Component.

```js
<Form.Group className="mb-3" controlId="formBasicPassword">
  <Form.Label>Image</Form.Label>
  <Form.Control type="file" placeholder="Image" />
</Form.Group>
```

2. Let's add an image and console log the event value to see what it gives us

```js
<Form.Group className="mb-3" controlId="formBasicPassword">
  <Form.Label>Image</Form.Label>
  <Form.Control
    type="file"
    placeholder="Image"
    onChange={(event) => console.log(event.target.value)}
  />
</Form.Group>
```

3. But since this is a file, it will be saved in `event.target.files`.

```js
<Form.Group className="mb-3" controlId="formBasicPassword">
  <Form.Label>Image</Form.Label>
  <Form.Control
    type="file"
    placeholder="Image"
    onChange={(event) => console.log(event.target.files)}
  />
</Form.Group>
```

As you can see it gives you an array, that's because this input field can take multiple images. But we only want one image in this case! So we need to take the first index of `event.target.files` only. The problem is that we use one method to handle all our input fields! So we will create another method that handles the image only.

4. Create a method called `handleImage` that saves the first index of the event's file.

```js
const handleImage = (event) =>
  setUser({ ...user, image: event.target.files[0] });
```

5. Pass the method to the input's `onChange`.

```js
<Form.Group className="mb-3" controlId="formBasicPassword">
  <Form.Label>Image</Form.Label>
  <Form.Control type="file" placeholder="Image" onChange={handleImage} />
</Form.Group>
```

6. When uploading an image to the backend, we saw that all of our data is saved in `req.body`, while the image is in `req.file`. To create an object that has both, we will use `FormData`, which is a built-in JavaScript class.

```js
const handleSubmit = async (e) => {
  e.preventDefault();
  const formData = new FormData();
  try {
    await axios.post('http://localhost:5000/users', user);
  } catch (error) {
    console.log(error);
  }
};
```

7. Now we will add all of our data in `newUser` to our instance `formData`. To do that we will loop over our `newUser` object and append every property in `newUser` to `formData`

```js
const handleSubmit = async (e) => {
  e.preventDefault();
  const formData = new FormData();
  for (const key in user) formData.append(key, user[key]);
  try {
    await axios.post('http://localhost:5000/users', user);
  } catch (error) {
    console.log(error);
  }
};
```

8. Now, pass formData to the post request.

```js
const handleSubmit = async (e) => {
  e.preventDefault();
  const formData = new FormData();
  for (const key in user) formData.append(key, user[key]);
  try {
    await axios.post('http://localhost:5000/users', formData);
  } catch (error) {
    console.log(error);
  }
};
```
