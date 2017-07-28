# Documentation of EmbeddedMontiArcMath

Publications
----
* [KRRvW17] E. Kusmenko, A. Roth, B. Rumpe, M. von Wenckstern:
  [Modeling Architectures of Cyber Physical Systems](http://www.se-rwth.de/publications/Systematic-Language-Extension-Mechanisms-for-the-MontiArc-Architecture-Description-Language.pdf).
  In: Modelling Foundations and Applications (ECMFA’17), 
      Held as Part of STAF 2017, pages 34-50. 
      Springer International Publishing, 2017.

This is only a short outline, please change it if it does not fit to your tutorial.
And please add additional points, if you need need them.
Every point should present exactly one feature and it should also contain minimal (but complete - model should be parseable) code examples (several code examples per points are possible, e.g showing different units).

EmbeddedMontiArc (Yannick)
----
* Introduction/Motivation.
* How to create a component?
* How to add ports to a component?
* How to instantiate a component?
* How to connect components?
* How to deal with component and port arrays?
* How to deal with generics and default generic values?
* How to deal with configuration parameters?

Math (Sascha)
----
* How to create a matrix?
  
  
* What Types of matrices are possible?
  
  
* How to deal with units? (compatiblity, conversion of units)
Currently all units will be automatically converted to the SI base units  
See https://en.wikipedia.org/wiki/SI_base_unit for reference  
An example of numbers with different units that can be added together is presented in the following:  
&nbsp;&nbsp;Q(O:5km) bigDistance = 4;  
&nbsp;&nbsp;Q(0:5m) smallDistance = 4;  
&nbsp;&nbsp;Q(0:10000m) sumDistance = bigDistance + smallDistance;  
  
This results in "sumDistance" containing 4004m. When changing the example to:  
&nbsp;&nbsp;Q(O:5km) bigDistance = 4;  
&nbsp;&nbsp;Q(0:5m) smallDistance = 4;  
&nbsp;&nbsp;Q(0:10km) sumDistance = bigDistance + smallDistance;  
  
"sumDistance" still contains 4004m as everything is converted to base units internally, which avoids manually converting  
units like km to m. Notice that in both cases "smallDistance" stores 4000m, as it is expected from the just explained  
internal unit conversions.   

* How to select values from a matrix?
  
* Listing of all supported opertions (+,*, ^, \, .*, .\, ...)
  
* Listing of all supported Octave functions (eig, diag, ...)

EmbeddedMontiArcMath (Sascha)
----
* How to embedded Math language into EmbeddedMontiArc?
  
In the EmbeddedMontiArc section several examples of how valid models look like were ilustrated.  
These models can be extended with capabilities from the Math Language by adding an "implementation Math" section.  
Consider the following EmbeddedMontiArc model:  

component Delay{  
&nbsp;&nbsp;ports in (0:1) in1,  
&nbsp;&nbsp;out (0:1) out1;  
           
}  

To use the Math Language an "implementation Math" section has to be added:  
component Delay{  
&nbsp;&nbsp;ports in (0:1) in1,  
&nbsp;&nbsp;out (0:1) out1;  
  
&nbsp;&nbsp;implementation Math{  
&nbsp;&nbsp;&nbsp;&nbsp;//Math code   
&nbsp;&nbsp;}  
}  


The delay components dataflow is modeled by the in1 and out1 ports, which take a value between 0 and 1.  
The intended functionality of the Delay component is to delay input for 1 tick, as can be guessed from the name.  
To achieve, this behaviour, the component can be enriched with that functionality by using the Math Language.  
This results in the following component:  

component Delay{  
&nbsp;&nbsp;ports in (0:1) in1,  
&nbsp;&nbsp;out (0:1) out1;  
          
&nbsp;&nbsp;implementation Math{  
&nbsp;&nbsp;&nbsp;&nbsp;static Q(0:1) delayValue=0; // default value on start  
&nbsp;&nbsp;&nbsp;&nbsp;out1=delayValue; //set output to value of last tick  
&nbsp;&nbsp;&nbsp;&nbsp;delayValue=in1; // store current tick value for next tick  
&nbsp;&nbsp;}  
}  

When looking at this example, inside of the implementation Math section, a static variable "delayValue" which has a value between  
0 and 1 is declared and initiliazed to 0. Then the "out1" port of the EmbeddedMontiArc component is set to take the value  
of "delayValue". Finally, "delayValue" is set to the current value of the "in1" port.   
Therefore, the Math Language can be used to add behaviour to a component which results in the ability of the  
EmbeddedMontiArcMath Language to defined components and their behaviour, and the ability to generate code out of these.  



To facilitate usage of ports which are definied in the EmbeddedMontiArc language, these ports are automatically adapted to be   
visible to the Math Language. If you are interested in how the language adaption exactly works you have to consults the  
MontiCore Language Workbench documentation.  
* How ports of EmbeddedMontiArc are adapted to matrices into Math language?  

EmbeddedMontiView (Fabian)
----
* What is the difference between EmbeddedMontiArc and EmbeddedMontiView?
* How to model abstract ports (with no name, with no data type and both)?
* What is an textual anonymous port ($port1)? 
* What are abstract connectors?
* What are abstract hierarchies?
* What are abstract effectors?

OCL (Ferdinand)
----
* How to define an invariant?
* What does context mean?
* How to use quantificators?
* How to deal with Lists, Sets?
* How to deal with implications, and other operators?
* What are OCL pre- and post conditions?
