## Increasing Heap size in Memory Analyzing Tool(MAT)

MAT is a great tool for analysing JVM heap dump. Usually while analysing heap dump, we get "Internal Error", this is beacuse MAT was not having enough memory to load the complete heap dump. MAT went OutOfMemory while loading a heap dump which was generated during out of memory :P.

To solve this we have to increase the heap size greater than the size of the heap dump we are trying to analyze. For this, you have to edit the _**MemoryAnalyzer.ini**_ file and change the -Xmx value. You will find this file in the root directory of MAT installation.

To set the max heap size value to 4GB, use
{% highlight ruby %}
-Xmx4g
{% endhighlight %}

If you are using MAT in Mac, you can see only the executable from which you launch MAT. In order to change the heap size head to the terminal and execute the below commands
{% highlight ruby %}
cd mat.app/Contents/Eclipse/
vim MemoryAnalyzer.ini
{% endhighlight %}
