# cocos_creator_mvvm_tools 0.1.1

### Release Notes
v0.1.0 - stable version of Typescript
v0.1.1 - Fix the wrong order of VMParent onLoad, and adjust the initialization time of some components

### Version Plan
Port Cocos Creator 3D 1.2 version, enhanced function
Port Cocos Creator 3.0 version (to be determined)

### Introduction

The mvvm tool set suitable for cocos creator, get rid of the way of traditional MVC setting node attributes to control UI. You can handle UI performance more quickly and in more detail. Complete complex display logic without writing a single line of code. The purpose of designing this framework is to solve the troublesome relationship between data and node state change switching.

### renew
Add JS call TS use case 2019/6/1

### Function

1. You don’t need to write any code, just set the parameters of the script component and you can get feedback immediately on the value of the monitoring path.
2. Get more flexible access to events when data changes, and realize various details that are difficult to handle.
3. Completely separate UI processing and game logic processing, and focus on business logic.

### Precautions

1. Difficulty in troubleshooting: Once the component is bound, it will become more difficult to find and investigate the bug location according to the situation. (also a disadvantage of componentization)
2. After designing the data model, you cannot change the attribute name at will. Once you need to modify it, you must open Cocos to modify it, which will be more dependent on editor operations. So a small component is provided internally to replace pathnames in batches.

### Project Structure

The core script files are stored in the assets\Script\modelView path, and all must be imported to use

- **JsonOb.ts** - implements the basic observer mode, changing the bound data will automatically call the callback function. You can always replace it with an observer you write yourself.
- **ViewModel.ts** - The core module of the VM, dynamically manages the ViewModel, and uses cc.director.emit to notify the node components in the game to change state.
- **VMBase.ts** - The core component of VM monitoring, used to receive the value change message of ViewModel. Derived components such as VMCustom /VMEvent are inherited from VMBase
- **VMParent.ts** - VM parent component, suitable for prefab pop-ups, it binds data to components that inherit VMparent, and only belongs to the instance created this time. Requires inherited use in a special way.

### Usage Notes

- Basic usage

   **import framework** - imports all scripts in assets\Script\modelView
   **Build Data Model** - Create a new data script anywhere, define your own data model, and use VM.add(data,'tag') to add viewModel. You can reference this data through the VM, or manage the data model globally yourself.
   **Hanging Script** - Add the component VMCustom directly in the editor, it will automatically identify the properties bound to the components and components that need to set values, such as cc.Label, cc.Progress, etc. As long as you fill in the corresponding watchPath, it will be automatically assigned to the property of the component. For example, fill in global.play.hp
   **Change Data** - Change the value of global.play.hp arbitrarily in the game, and the corresponding label will automatically change the value.

- Register VM globally: (Global free use path)
    VM.add(data,'tag'); //

- Local components use VM: (relative paths used only within components)
   1. Inherit the VMParent component
   2. Set data data (data attribute) in the component
   3. Relative path Use *.name to set watchPath, VMParent will automatically replace * with the actual ViewModel label when onLoad, so as to monitor data changes.

### Help instructions

Please refer to the attached document for specific usage: /docs

[Usage Documentation] (https://github.com/wsssheep/cocos_creator_mvvm_tools/tree/master/docs)
