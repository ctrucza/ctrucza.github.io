---
layout: post
title:  "Drive-by refactoring: Exceptions and Files"
date:   2015-01-09 18:01:00
categories:
---

Consider the following code:

{% highlight Java %}
private static void writeIDToFile(DeviceID deviceID, File file) {
    ObjectOutputStream oos = null;
    try {
      file.createNewFile();
      oos = new ObjectOutputStream(new FileOutputStream(file));
      oos.writeObject(deviceID);
      oos.flush();
    }
    catch (Exception e) {
      // TODO handle?
    }
    finally {
      if (oos != null) {
        try {
          oos.close();
        }
        catch (IOException e) {
          // ignore
        }
      }
    }
  }
{% endhighlight %}

First issue: 

{% highlight Java %}
catch(Exception e) { 
    // TODO handle?
}
{% endhighlight %}

Rule:
> never catch an exception you can't handle

In the current context there is no meaningful action we can take on the exception. It can be caused by failure to create the file, writing the object or flushing the written data. We could just ignore the exception:

{% highlight Java %}
private static void writeIDToFile(DeviceID deviceID, File file) {
    ObjectOutputStream oos = null;
    try {
      file.createNewFile();
      oos = new ObjectOutputStream(new FileOutputStream(file));
      oos.writeObject(deviceID);
      oos.flush();
    }
    finally {
      if (oos != null) {
        try {
          oos.close();
        }
        catch (IOException e) {
          // ignore
        }
      }
    }
  }
{% endhighlight %}

Second issue: the finally block also contains an un-handlable exception, which we can ignore:

{% highlight Java %}
private static void writeIDToFile(DeviceID deviceID, File file) {
    ObjectOutputStream oos = null;
    try {
      file.createNewFile();
      oos = new ObjectOutputStream(new FileOutputStream(file));
      oos.writeObject(deviceID);
      oos.flush();
    }
    finally {
      if (oos != null) {
          oos.close();
        }
      }
    }
  }
{% endhighlight %}

Third issue: the `finally` block is still ugly. We close the stream if it was opened. But if it wasn't opened (because of an exception) why would we get into the finally block at all?

{% highlight Java %}
private static void writeIDToFile(DeviceID deviceID, File file) {
    file.createNewFile();
    ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(file));
    try {
      oos.writeObject(deviceID);
      oos.flush();
    }
    finally {
        oos.close();
    }
  }
{% endhighlight %}
