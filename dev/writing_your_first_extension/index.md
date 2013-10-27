---
layout: up
title: Writing Your First Extension
---

# Writing Your First Extension

[PyPN](http://www.pnotepad.org/add-ons/) provides a great way to add functionality to Programmer’s Notepad by writing simple Python code, but you might want to do something more advanced. For this there’s the Programmer’s Notepad [Extension SDK](http://www.pnotepad.org/developer/).

The SDK lets you extend PN using C++, allowing you to react to editor events and provide new commands in the menu. PyPN is itself implemented as an extension using this same SDK, and you can use the SDK to provide support for other scripting languages too.

## What You’ll Need

You need a Windows C++ compiler and the [Boost](http://boost.org/) C++ library. Note that you don’t need to compile any of boost, we use the header-only bits.

I suggest using the free [Microsoft Visual C++ Express](http://www.microsoft.com/express/vc/Default.aspx) if you don’t already have Visual Studio, this should guarantee compatibility.

## Getting Started

Download the SDK and copy the template project, this is a good base for your extension. Note that the SDK also contains a demo extension showing use of various parts of the SDK. Change the name and version of your extension and you’re ready to add it to Programmer’s Notepad for the first time:

```cpp
void __declspec(dllexport) __stdcall pn_get_extension_info(PN::BaseString& name, PN::BaseString& version)
{
    name = "My First Plugin";
    version = "1.0";
}
```

Compile the extension and place the .dll file in your PN directory. Now run “pn --findexts” and your plugin will be discovered and loaded the next time you start PN. Go to Tools-&gt;Options-&gt;Extensions and see your extension listed.

Everything else you want to do flows from the instance of IPN that’s passed to your init function. This interface gives you access to the open documents, lets you sign up to handle document events and gives you access to app-level services like script registration, find in files and options management.

## Working with Documents

Everything you want to do with an open document is done through the IDocument interface. You get a pointer to one of these from your IPN instance by calling GetCurrentDocument, NewDocument or equivalent.

```cpp
    // Make a new document
    IDocumentPtr doc = pn->NewDocument(NULL);
    
    // Send scintilla messages (see scintilla.org documentation)
    doc->SendEditorMessage(SCI_APPENDTEXT, 6, (LPARAM)"Hello!");

    // Save changes
    doc->Save("c:\\temp\\test.txt", true);

    // Done with the document
    doc->Close();
```

## Adding Menu Items

Menu items are added via the IPN object provided at extension load time. You add menu items by providing an implementation of IMenuItems. Here is a very simple implementation supporting a single item:

```cpp
class Menu : public extensions::IMenuItems
{
public:
    Menu()
    {
        memset(&amp;item1, 0, sizeof(item1));
        item1.Handler = &amp;MyExtensionFunc;
        item1.Title = L&quot;Hello World&quot;;
        // item1.Type = extensions::miItem;
        // item1.UserData = 0;
    }

    /**
     * Get the number of MenuItem instances that can be retrieved.
     */
    virtual int GetItemCount() const
    {
        return 1;
    }

    /**
     * Get an individual MenuItem instance by index.
     */
    virtual extensions::MenuItem&amp; GetItem(int index) const
    { 
        if (index == 0)
            return const_cast<extensions::menuitem&>(item1);
    }

private:
    extensions::MenuItem item1;
};
```

This very simple implementation adds a single menu item called &quot;Hello World&quot;. When the item is selected, the MyExtensionFunc method will be executed by PN.

It might seem like a fair bit of code to add a menu item, but in the next SDK version I'll provide a general-purpose helper to make this easy. The reason for the interface is to make it easy for you to provide sub-menus. MenuItem instances can contain child items by changing Type to miSubItem and filling in the SubItems member with another instance implementing IMenuItems.

The handler method looks like this:

```cpp
void MyExtensionFunc(extensions::cookie_t /*cookie*/)
{
    g_PN->GetGlobalOutputWindow()->AddToolOutput("Hello World!");
    g_PN->ShowOutput();
}
```

Once you have your handler and IMenuItems implementation, you just need to tell PN about your items. In your init function you need something like this:

```cpp
bool __stdcall pn_init_extension(int iface_version, extensions::IPN* pn)
{
    if(iface_version != PN_EXT_IFACE_VERSION)
        return false;

    g_PN = pn;

    Menu menu;
    pn->AddPluginMenuItems(&menu);

    return true;
}
```

Now you have Programmer's Notepad presenting your commands in the menu and can handle their selection.