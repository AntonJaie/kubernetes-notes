# Labels 
- key-value pairs attached to K8s object
- used to organize and select a subset of objects. meaning several objects can have the same label(s)


## Label Selectors
- used to select a subset of objects

### Equality-Based Selectors
- uses **=** or **==** and **!=** operators
- Example
```
> env == dev
> env = dev
```

### Set-Based Selectors
- uses **in**, **notin**, **exist**, and **does not exist**
- Example
```
> env in (dev, qa)
```   