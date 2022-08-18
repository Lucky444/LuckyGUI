# LuckyGUI
A simple Spigot based GUI-library

__Disclaimer: This isn't a finished project, and currently the library has some unecessery features__

To use, you'll have to create a new GUIManager in your onEnable method

You can create new classes which implement the GUI class, leaving you to override three methods:
```
@Override
public void onOpen(Player user)
```
- Gets called when the GUI is openend for a player
```
@Override
public void onClose()
```
- Gets called when the GUI is closed  

```
@Override
public void onClick(int slot)
```
- Gets called when the player clicks an item in the GUI


On top of that, you can also __style__ the GUI with three functions (you should call them in your constructor in the following order):
```
setSize(int size)
```
- Sets the inventory size (only multiples of  will work)

```
setName(String name)
```
- Sets the name of the GUI

```
construct()
```
- Has to be called to apply changes to the name and the size of the GUI

```
fill(ItemStack item)
```
- Fills the entire GUI with the given Item

```
set(ItemStack item, int slot)
```
- Sets the item at the given slot to the given item


A complete GUI object might look something like this:
```
class myNewGUI implements GUI{

  public myNewGUI(){
    constructView(null);
  }
  
  private void constructView(Player p){
    setSize(9);
    setName(p == null ? "TMP" : p.getName());
    construct();
    fill(backgroundItem);
    set(ConfirmButton, 0);
    set(CancelButton, 8);
  }

  @Override
  public void onOpen(Player p){
    constructView(p);
  }
  
  @Override
  public void onClose(){}
  
  @Override
  public void onClick(int slot){
    switch(slot){
      case(0):
        confirm();
        break;
        
      case(9):
        cancel();
        break;
    }
  }
}
```

Before opening this GUI however, you'll have to create a GUIManager, with a plugin instance
```
GUIManager manager = new GUIManager(this);
```
Since this GUIManager is written in a Singleton pattern, it's enough to create it once in the onEnable method of your plugin

When you want to open a GUI object for a player, you'll have to create a new objetc of this GUI type first:
```
myNewGUI toOpen = new myNewGUI();
```

Then you can open this **specific** GUI object for the player, keep in mind, that you should only open one specific object for each player
```
toOpen.open(player);
```
