# How to use

Simply add the repository to your build.gradle file:

```
repositories {
        maven { url 'https://raw.githubusercontent.com/jarlen/maven-repo/master/' }
        jcenter()

    }
```

And you can use the artifacts like this:

```
dependencies {
    ......
    compile 'cn.jarlen.maven:mvp:0.0.0.1'
    ......
}
```


#License:

The licenses are provided by the individual libary owner. This "maven-repo" utilizes these libaries and won´t provide any support, responsibility or licenses itself.
