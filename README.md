# Grapevine :grapes:

Grapevine is the reenvention of a Dependency Injection (DI) container. It is solely inspired on [Spring](https://spring.io/docs) and [PicoContainer](http://picocontainer.com/introduction.html). The main idea of the project is to identify user-tagged objects and dependencies, structure them and finally return instances of the objects based on a user's criteria.

The name comes from an extended metaphor created by the developers of this project in order to understand DI from a simpler point of view. In simple terms, a grapevine consists of many grapes, and each grape consists of many seeds. Assuming a program is a grapevine, the following abstractions come to mind:

 * **Grape:** this represents an object that can be managed by the container.
 * **Seed:** a component inside of a grape, which in this case is a dependency. 



## Using Grapevine

### XML File

Continuing with the abstraction, since grapes and seeds have a 1:N relationship between them, this means that a tree-like structure might be appropriate to declare objects and dependencies. The following is an example of how a Grapevine Context XML File would look like.

```xml
<grapes>
	<grape id = "lightTestSetter" class = "LightTestSetter" scope = "singleton" init-method = "init" property="type"/>
	<grape id = "lightTestConstructor" class = "LightTestConstructor" scope = "singleton" init-method = "init">
		<seed name="switch" type="String" constructor="true" isReferenced="false" value="bOOOM2"/>
	</grape>
</grapes>

```

Each grape can have more than one seed, and preexisting objects can be noted with the *isReferenced* tag. Each grape and seed has a unique identifier. Autowiring is done with the *property* tag, and can be either by value or by name. Just like its predecessor containers, *scope*, *class* and init/destroy methods (*init-method*, *destroy-method* in our nomenclature).

### Annotations

The equivalent can be made straight from the source code. As opposed to Spring, Annotations cannot be mixed with XML and viceversa. There are five main annotations:

* **GrapeAnnotation:** It's a class annotation used to mark which which objects are grapes. 
* **AutowireGrape:**  Marks all methods (dependencies) of a specific Grape.
* **InitMethod:** Method called immediately after a grape is initialized.
* **DestroyMethod:** Method called immediately before a grape is destroyed.
* **Scope:** Just like Spring, a user can decide if only one object instantiation is made (singleton), or several. 

Here is an example of a what a class would generally look like:

```java

package Examples;

import Annotations.*;

@GrapeAnnotation
public class EtekcityTest {

    private String switch;
    
    /*the following autowire is done by name. If no id is placed, 
    the container will automatically detect the calling as by type.*/
    @AutowireGrape(id="theState") 
    public void setState(String state) {
        switch = state;
    }
    
    public String getState() {
        return switch;
    }
    
    @InitMethod
    public void init(){
    	System.out.println("Alejandra the rat. ");
    }
    
}

```




### Running the program

In order to run the program, a new instance of *GrapevineContext* has to be declared. If the user is going to use an XML file to obtain all objects and dependencies, the inicialization should be with *XMLGrapevineContext*, with the XML file path sent as a parameter. In case Annotations are used, the project should be in a single contained package and should be sent as a parameter to a new *AnnotationsGrapevineContext*. The only method that is available when manipulating objects in the Grapevine container is *getGrape(id)*, which is in charge of retrieving a grape's contained instance by its identifier.

```java

import Context.*;

import Examples.*;

public class Main {
   
   public static void main(String[] args) {
        GrapevineContext ctx = new XMLGrapevineContext("Prueba.xml");
		LightTestSetter grape = (LightTestSetter) ctx.getGrape("lightTestSetter");
        grape.init();

    }

}

```

## Constraints and Details

The project is not 100% functional, and has some bugs to be fixed (especially in the Annotations section). Besides from the source code, inside the project there is a User's Manual with a detailed description of each of the container's components (in JavaDoc format). There is also a package with implemented examples and a Main class to test the program's functionality. This is project is also [on GitHub](https://github.com/paolaos/grapevine) and, even though it's certainly finished, it could be improved and optimized.


### Creators:
* Paola Ortega Saborío
* Gonzalo Ramírez
* Esteban Alemán

*Universidad de Costa Rica, II-2017*
