# JaMeLeA - Java Memory Leak Analysis 

Command line tool to suspect the memory leak in java by analyzing JCMD class histogram files.

Script will calculate the difference between two jcmd class histograms and displays top N leak suspects sorted by percentage growth.

Note: originally implemented with java coding standards later partially converted to python coding formats. please ignore code formatting. 
# Using this tool

Java process configuration
1. Add -XX:+UnlockDiagnosticVMOptions JVM option to java process to detect memory leak (optional)
2. Run testcase/action (or first histogram before any test)
3. Run forceful GC using JCMD `jcmd <pid> GC.run` 
4. Run `jcmd  <pid> GC.class_histogram -all > /data/dump/histogram1.txt`
5. Run `jcmd <pid> GC.heap_dump /data/dump/dump1.hprof`
6. Run testcase/action
7. Run forceful GC again `jcmd <pid> GC.run` 
8. Run `jcmd <pid> GC.class_histogram -all > /data/dump/histogram2.txt`
9. Run `jcmd <pid> GC.heap_dump /data/dump/dump2.hprof`

After collecting the histogram files run the script `python jamelea.py -f histogram1.txt -s histogram2.txt` to get the suspect report.

Report customizations
1. Top N Suspects report `python jamelea.py -f histogram1.txt -s histogram2.txt --top 5`
2. Package filtered reports, two filters available 'xcompany only' and '3rd party only' 
    To filter your company specific objects add xcompany package identifier xcompany RegEx list in script
    `python jamelea.py -f histogram1.txt -s histogram2.txt --top 5 3rdponly` or `python jamelea.py -f histogram1.txt -s histogram2.txt --top 5 xcompanyonly`


# References:
- https://stackoverflow.com/a/2511709
- https://stackoverflow.com/questions/28407307/how-to-clear-objects-hashmap-to-be-garbage-collected-java
- https://www.ej-technologies.com/blog/2017/03/finding-a-memory-leak-with-jprofiler/
