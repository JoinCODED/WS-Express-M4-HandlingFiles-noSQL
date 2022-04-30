1. Express can't directly upload files and images, which are considered `multipart/form-data`. So we will use [`multer`](https://www.npmjs.com/package/multer), which is a middleware responsible for uploading files.

2. Let's start with installing it:

   ```shell
   $ npm install multer
   ```

3. Now we need to do a few configurations such as the destination and the name of the file we're saving. In `api/users/users.routers.js`, we will require `multer` and define a variable called `storage` which is equal to the return value of `multer.diskStorage` which is a method that gives us full control on storing files.

   ```javascript
   const multer = require('multer');

   const storage = multer.diskStorage();
   ```

4. `diskStorage` takes an object as an argument, and this object has two properties `destination` which is where the image will be saved and `filename` which is what we want to call our image. We will save our images in `./media`, note that the path is according to the main file which is `app.js` not according to `api/users/users.routers.js`.

   ```javascript
   const multer = require('multer');

   const storage = multer.diskStorage({
     destination: './media',
     filename: '',
   });
   ```

5. `filename` takes a function that has three arguments, the request, uploaded file and a callback function which we will trigger in `filename`'s function. The callback function `cb` takes two arguments, an error and the new name of the uploaded file. We will set the error to null, and for the name of the uploaded image we will set to the current date (to have a unique name even if the image is uploaded more than one time) followed by the original name of the image.

   ```javascript
   const storage = multer.diskStorage({
     destination: './media',
     filename: (req, file, cb) => {
       cb(null, `${+new Date()}${file.originalname}`);
     },
   });
   ```

6. Next, we will create a variable called `upload` and pass it the return value of `multer` which is a middleware method that will enable image uploading.

   ```javascript
   const upload = multer({
     storage,
   });
   ```

7. Now we need to pass the `upload` to the routes that will need to upload images, which are the user create and update routes.

8. We must specify if one or more images can be uploaded. In our case, every item has 1 image so we will use the method `single` and pass it the name of the field we want to save the image in, which is `image`. We will pass it as a middleware that will run before the controller methods:

```javascript
// User Create
router.post('/', upload.single('image'), userCreate);

// User Update
router.put('/:postId', upload.single('image'), userUpdate);
```
