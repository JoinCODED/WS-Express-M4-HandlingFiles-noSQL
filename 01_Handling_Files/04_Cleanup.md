Let's clean up our code a little bit and move the file uploading middleware to its file.

1. Create a folder called `middleware`.

2. Inside it create a file called `multer.js`.

3. Move all the code related to image handling to it. Don't forget to `export` upload.

   ```javascript
   const multer = require('multer');

   const storage = multer.diskStorage({
     destination: './media',
     filename: (req, file, cb) => {
       cb(null, `${+new Date()}${file.originalname}`);
     },
   });

   // Initialize upload variable
   const upload = multer({
     storage,
   });

   module.exports = upload;
   ```

4. In `api/users/users.routers.js`, `require` `upload`.

   ```javascript
   const upload = require('../middleware/multer');
   ```

Our route page looks much cleaner now!
