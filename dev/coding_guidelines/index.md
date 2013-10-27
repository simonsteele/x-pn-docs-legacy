---
layout: up
title: Coding Guidelines
---

# Some Code Guidelines

Here are some work-in-progress guidelines for the C++ used in Programmer's Notepad. Some are not followed across the whole code base, although efforts are being made to improve this.

### Naming

Public methods are named LikeThis(), private/protected methods likeThis(). Member variables have the `m_` prefix, static variables have `s_`.

Generic WTL UI classes are prefixed with C, like CMySomethingView. All other classes are not prefixed so LikeThis.

### Allocation / Smart Pointers

Generally use smart pointers (`boost::shared_ptr`) where objects are to be passed around, this avoids complicated ownership issues.

### Containers / STL Use

Prefer `vector<T>` for storing flat collections, unless you have a specific performance reason to do otherwise. For associative storage prefer `map<K, V>` unless your specific case would benefit from hash lookups.
