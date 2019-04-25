# Description

[XMLInputFormat](https://github.com/apache/mahout/blob/ad84344e4055b1e6adff5779339a33fa29e1265d/examples/src/main/java/org/apache/mahout/classifier/bayes/XmlInputFormat.java) is an implementation that reads records which are delimited by a specific begin/end tag. This is useful when providing logical file splits to the mapper based on a single start and end tag.

Consider the following XML

```xml
<catalog>
   <book>
      <author>Gambardella, Matthew</author>
      <title>XML Developer's Guide</title>
      <genre>Computer</genre>
      <price>44.95</price>
      <publish_date>2000-10-01</publish_date>
      <description>An in-depth look at creating applications 
      with XML.</description>
   </book>
   <book>
      <author>Ralls, Kim</author>
      <title>Midnight Rain</title>
      <genre>Fantasy</genre>
      <price>5.95</price>
      <publish_date>2000-12-16</publish_date>
      <description>A former architect battles corporate zombies, 
      an evil sorceress, and her own childhood to become queen 
      of the world.</description>
   </book>
   <book>
      <author>Corets, Eva</author>
      <title>Maeve Ascendant</title>
      <genre>Fantasy</genre>
      <price>5.95</price>
      <publish_date>2000-11-17</publish_date>
      <description>After the collapse of a nanotechnology 
      society in England, the young survivors lay the 
      foundation for a new society.</description>
   </book> .....
</catalog>
```

Set the `start_tag` as `<book>` and `end_tag` as `</book>` for the XMLInputFormat like this :

```java
Configuration conf = new Configuration();
conf.set(XmlInputFormat.START_TAG_KEY, "<book>");
conf.set(XmlInputFormat.END_TAG_KEY, "</book>");
....
```

```java
Job job = new Job(conf, "XML Processing Processing");
job.setInputFormatClass(XmlInputFormat.class);
....
```


This will ensure that every mapper gets one logical entity irrespective of the physical split of the file on HDFS. *[Of course the XML file on HDFS must be well-formd]*


However this is not useful when you would like to extract records based on multiple start and end tags.

Consider the following XML

```xml
<catalog>
   <book>
      <author>Gambardella, Matthew</author>
      <title>XML Developer's Guide</title>
      <genre>Computer</genre>
      <price>44.95</price>
      <publish_date>2000-10-01</publish_date>
      <description>An in-depth look at creating applications 
      with XML.</description>
   </book>
   <book>
      <author>Ralls, Kim</author>
      <title>Midnight Rain</title>
      <genre>Fantasy</genre>
      <price>5.95</price>
      <publish_date>2000-12-16</publish_date>
      <description>A former architect battles corporate zombies, 
      an evil sorceress, and her own childhood to become queen 
      of the world.</description>
   </book>
   <book>
      <author>Corets, Eva</author>
      <title>Maeve Ascendant</title>
      <genre>Fantasy</genre>
      <price>5.95</price>
      <publish_date>2000-11-17</publish_date>
      <description>After the collapse of a nanotechnology 
      society in England, the young survivors lay the 
      foundation for a new society.</description>
   </book> 
   <article>
      <author>Corets, Eva</author>
      <title>Oberon's Legacy</title>
      <genre>Fantasy</genre>
      <price>5.95</price>
      <publish_date>2001-03-10</publish_date>
      <description>In post-apocalypse England, the mysterious 
      agent known only as Oberon helps to create a new life 
      for the inhabitants of London. Sequel to Maeve 
      Ascendant.</description>
   </article>
   <paper>
      <author>Corets, Eva</author>
      <title>The Sundered Grail</title>
      <genre>Fantasy</genre>
      <price>5.95</price>
      <publish_date>2001-09-10</publish_date>
      <description>The two daughters of Maeve, half-sisters, 
      battle one another for control of England. Sequel to 
      Oberon's Legacy.</description>
   </paper>
   <article>
      <author>Randall, Cynthia</author>
      <title>Lover Birds</title>
      <genre>Romance</genre>
      <price>4.95</price>
      <publish_date>2000-09-02</publish_date>
      <description>When Carla meets Paul at an ornithology 
      conference, tempers fly as feathers get ruffled.</description>
   </article>
   <paper>
      <author>Thurman, Paula</author>
      <title>Splish Splash</title>
      <genre>Romance</genre>
      <price>4.95</price>
      <publish_date>2000-11-02</publish_date>
      <description>A deep sea diver finds true love twenty 
      thousand leagues beneath the sea.</description>
   </paper>
   
   .....
</catalog>
```

If you want to grab all the those entities within `<book></book>`, `<article></article>`,`<paper></paper>` use this [XMLInputFormatWithMultipleTags](https://github.com/Mohammed-siddiq/hadoop-XMLInputFormatWithMultipleTags/blob/master/XmlInputFormatWithMultipleTags.java) as :

```java
Configuration conf = new Configuration();
conf.set(XmlInputFormatWithMultipleTags.START_TAG_KEYS, "<book>,<article>,<paper>");
conf.set(XmlInputFormatWithMultipleTags.END_TAG_KEYS, "</book>,</article>,</paper>");
```

***Note that multiple tags are delimeted by a comma (,)*** 

```java
Job job = new Job(conf, XmlInputFormatWithMultipleTags.class);
....
```
