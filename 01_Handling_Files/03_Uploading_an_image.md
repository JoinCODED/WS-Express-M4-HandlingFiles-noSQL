1. Let's make a request to user create and log the request to see what our data looks like.

2. But how can we upload an image on Postman? In body, instead of using `raw` we will use `form-data`. The fields will be the `key`, and the values in `value`. When adding the `image` field, hover over it and you'll see that you can choose the type of data, change it to `file`. In `value`, click on `Select Files` and choose your image.

3. As you can see in the console, the image is not saved inside `req.body`, instead it's inside `req.file`.

4. So what we'll do is save the image in `req.body` so that it gets passed to the database. But what we want to pass is the path we created at the beginning:

   ```javascript
   try {
     req.body.image = `http://localhost:8000/media/${req.file.filename}`;
     const newUser = await User.create(req.body);
     res.status(201).json(newUser);
   }
   ```

5. But what happens when we deploy our backend? It will no longer be a local host! To fix that we will get the protocol and host from the request, followed by the name of the image from `req.file`.

   ```javascript
   try {
     req.body.image = `http://${req.get("host")}/media/${req.file.filename}`;
     const newUser = await User.create(req.body);
     res.status(201).json(newUser);
   }
   ```

6. What if the front end didn't pass an image? `req.file` will be undefined, so we can add a condition:

   ```javascript
   try {
     if (req.file) {
       req.body.image = `http://${req.get("host")}/media/${req.file.filename}`;
     }
     const newUser = await User.create(req.body);
     res.status(201).json(newUser);
   }
   ```

7. In the same way, we can update the user update route:

   ```javascript
   try {
     if (req.file) {
       req.body.image = `http://${req.get("host")}/media/${req.file.filename}`;
     }
     await User.findByIdAndUpdate(req.body.id,req.body);
     res.status(204).end();
   }
   ```

8. Let's test it out!
