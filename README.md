# file-locker

### What it does
FileLocker is a drastic improvement over the nearly unusable FileSystem API. It exposes methods to easily create and list both directories and files, both text-based and images, as well as providing functions for storing, encrypting, and decrypting sensitive data that needs encryption.

### Initialization
```
var locker = new FileLocker();
```
This will initialize a temporary FileSystem. The downsides of using the temporary file system is that the browser may delete these files at discretion later on. To initialize a persistent file system which will not be deleted in perpetuity use:
```
var locker = new FileLocker(true);
```
When using the persistent file system, after initialization, if you have not granted the current domain access to the file system before it will pop up requesting the permissions necessary to work with the API. Clicking "block" instead of "allow" obviously will result in not being able to access the file system api.

Its also possible to specify the amount of bytes you want to request for the file system allocation by passing in a second argument. The default is 5 * 1024 * 1024 (5MB).
```
var locker = new FileLocker(false, 1 * 1024 * 1024);
```

### Usage

##### Create a directory
```
locker.createDirectory('/kittens', response => console.log(response));
```
##### Remove a directory
```
locker.removeDirectory('/kittens', response => console.log(response));
```
##### List contents of a directory
```
locker.listDirectory('/kittens', response => console.log(response));
```
##### Save a file
```
var js = `
function go(){
  console.log('GO');
}`

locker.saveFile('/kittens/go.js', js, response => console.log(response));
`;
```

##### Load a file
```
locker.loadFile('/kittens/go.js', response =>
{
  var script = document.createElement('script');
  script.innerHTML = response;
  document.body.appendChild(script);
});
```

##### Remove a file
```
locker.removeFile('/kittens/go.js', response => console.log(response));
```

##### Save an image
```
locker.saveImage('/kittens/kitty','<url to picture of kittens>', response => console.log(response));
```

##### Load an image
```
locker.loadImage('/kittens/kitty', response => console.log(response));
```

##### Encrypt and Decrypt files
```
let iv;
locker.saveEncryptedFile('/kittens/secret_kitty','something confidential to save','<password here>', response =>
{
  console.log(response); // this is the unique iv. You will need this to decrypt the file.
  iv = response;
});

locker.loadEncryptedFile('/kittens/secret_kitty','<password here>', iv, response => console.log(response));
```
