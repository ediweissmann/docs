### fork

The `fork` command copies an image to another repository on your local machine. 

```
Usage: spoon fork <options> <image> [<repository>/]<image>[:<tag>]

<options> available:
      --overwrite            Overwrite existing image
      --wait-after-error     Leave program open after error
```

If the repository specified in the command does not already exist, a new one is automatically created.  

```
# Copy spoonbrew/node to a new repository
> spoon fork spoonbrew/node my-node

Output image: my-node:head

# Fork the image to the existing repository with a new tag
> spoon fork spoonbrew/node my-node:1.0
```