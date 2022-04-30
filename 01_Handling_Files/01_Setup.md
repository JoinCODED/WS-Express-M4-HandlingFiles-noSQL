We will work on this Registration Api, fork and clone [this](https://github.com/JoinCODED/Demo-Express-M4-HandlingFiles-noSQL) in your development folder.

1. The first step is creating a `media` folder for all our images.

2. Let's add a random image to our `media` folder and see how we can access it.

3. To access this image we will need the directory path. We can access it using `__dirname`. Let's log it to get the project's directory path:

   ```javascript
   console.log('__dirname ', __dirname);
   ```

4. We'll copy the directory name, paste it in the web browser's url followed by `/media/<image_name>`. And the image will appear!

5. But that's not how we should access our images! We should access them from the server which in this case is the local host. This is what we want the path of the images to be:

   `http://localhost:8000/media/<image_name>`

6. To do that we'll create a route that links all the images in the `media` folder to the link above. First, we join the directory path with `media` which is the location of the directory. Let's console log it to see how `path.join` works.

   ```javascript
   const path = require("path");

   [...]

   console.log("hii", path.join(__dirname, "media"));
   ```

7. Then we pass `path.join` to a middleware method. Place it under your user routes.

   ```javascript
   app.use('/users', usersRoutes);
   app.use(path.join(__dirname, 'media'));
   ```

8. Then we use `express.static()`, which is a function that takes a path, and returns a middleware that serves all files in that path to `/`.

   ```javascript
   app.use('/media', express.static(path.join(__dirname, 'media')));
   ```

9. Let's test our image now with the following path:

   `http://localhost:8000/media/<image_name>`
