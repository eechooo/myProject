---
layout: post
title:  "Welcome to Jekyll!"
date:   2014-11-17 13:31:01 +0800
categories: jekyll
tag: jekyll
---

* content
{:toc}


{% highlight Java %}
// package whatever; // don't place package name!

import java.io.*;
import java.util.*;

class MyCode {
    public static void main (String[] args) {
        System.out.println("Hello Java");
    Location location1 = new Location("Los Angeles", 1);
    Location location2 = new Location("San Jose", 2);
    List<Location> locations = new ArrayList<Location>();
    locations.add(location1);
    locations.add(location2);
    for(Location location: locations) {
      System.out.println(location.name);
    }
    LocationStore ls = new LocationStore(locations);
    ls.findLocation("Los Angeles");
    }
}


class Location {
  public int id;
  public String name;
  public Location parent;

  public Location(String name, int id) {
    this.name = name;
    this.id = id;
  }
}

class CouponStore {

  class Coupon {
    public int id;
    public String subject;
    public int priority;
    public Location location;
  }

  public List<Coupon> coupons;
  public LocationStore ls;
  public HashMap<String, List<Coupon>> CityNameToCouponMap;

  public CouponStore(List<Coupon> coupons, LocationStore ls) {
    CityNameToCouponMap = new HashMap<>();

    for (Coupon coupon: coupons) {
      String cityName = coupon.city;
      if (!CityNameToCouponMap.containsKey(cityName)) {
        coupon.get(cityName).add(coupon);
      } else {
        List<Coupon> couponList = new ArrayList<>();
        couponList.add(coupon);
        map.put(cityName, couponList);
      }
    }
  }

  // public Coupon findCoupon(String location) {
  //   Location lo = ls.findLocation(location);
  //   String cityName = lo.name;
  //   String citysParent = lo.parent;
  //   if (map.containsKey(cityName)) {
  //     return map.get(cityName);
  //   } else {
  //     while (lo.parent != null) {
  //       Location parent = lo.parent;
  //       if (map.containsKey(parent.name)) {
  //           return map.get(parent.name)
  //       }
  //     }
  //   }
  //   return null;
  // }

  public List<Coupon> findCoupon(String location) {
    Location lo = ls.findLocation(location);
    if (lo == null) {
      return null;
    }

    String cityName = lo.name;
    Location citysParent = lo.parent;

    if (CityNameToCouponMap.containsKey(cityName)) {
      return CityNameToCouponMap.get(cityName);
    } else {
      String parentCity = citysParent.name;
      return findCoupon(parentCity);
    }
  }

 // Locations:
  // {id: 0, name: "CA", parent: null}
  // {id: 1, name: "Los Angeles", parent: CA}
  // {id: 2, name: "Santa Monica", parent: LA}
  // Coupon:
  // {id: 0, location: LA, priority: 1} 1 ~ 5
  // {id: 1, location: LA, priority: 3}
  //
}


class LocationStore {

  public List<Location> locations;
  public HashMap<String, Location> cityNameToLocationMap;

  public LocationStore(List<Location> locations) {
    this.locations = locations;
    cityNameToLocationMap = new HashMap<>();
    for (Location location : locations) {
      String cityName = location.name;

      cityNameToLocationMap.put(cityName, location);
    }
  }

  public Location findLocation(String name) {
    if (cityNameToLocationMap.containsKey(name)) {
      System.out.println(cityNameToLocationMap.get(name));
      return cityNameToLocationMap.get(name);
    } else {
      return null;
    }
  }
}

{% endhighlight %}


{% highlight Java %}

// package whatever; // don't place package name!

import java.io.*;
import java.util.*;

class MyCode {
    public static void main (String[] args) {
        System.out.println("Hello Java");
    Iterator it = new Iterator("McLeffy");

    while (it.hasNext()) {
      it.next();
      it.remove();
      System.out.println(it.strs);
    }
    }
}


class Iterator {

  public String strs;
  public int cursor;
  public int size;
  public int lastPosition;

  public Iterator(String strs) {
    this.strs = strs;
    this.size = strs.length();
    this.lastPosition = -1;
  }

  public Boolean hasNext() {
    return cursor != size;
  }

  public char next() {
    int i = cursor;
    size = strs.length();
    while (i < size) {
      char ch = strs.charAt(i);
      if (Character.isUpperCase(ch)) {
        cursor = i;
        lastPosition = i;
        System.out.println(ch);
        return ch;
      }
      i += 1;
    }
    if(i >= size) {
      throw new NoSuchElementException();
    }
    return 'a';
  }

  // public Boolean hasNext() {
  //   int j = index
  //   while (j < strs.length()) {
  //     char ch = strs.charAt(j)
  //     if (Character.isUpperCase(j)) {
  //       this.index = j - 1
  //       return true
  //     } else {
  //       j += 1
  //     }
  //   }
  // }

  // public char next() {
  //   j += 1
  //   return strs.charAt(j)
  // }

  public void remove() {
    if (lastPosition < 0) {
      throw new IllegalStateException();
    }
    lastPosition = -1;
    String copyedStrs = strs;
    this.strs = copyedStrs.substring(0, cursor) + copyedStrs.substring(cursor + 1);
  }
}


{% endhighlight %}

First POST build by Jekyll.


诫子书				{#zhugeliang}
------------------------

![诫子书]({{ '/styles/images/jiezishu.jpg' | prepend: site.baseurl  }})


[诸葛亮](#)


夫君子之行，静以修身，俭以养德。非淡泊(澹泊)无以明志，非宁静无以致远。夫学须静也，才须学也。非学无以广才，非志无以成学。淫慢则不能励精，险躁则不能冶性。
年与时驰，意与日去，遂成枯落，多不接世，悲守穷庐，将复何及！


[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
