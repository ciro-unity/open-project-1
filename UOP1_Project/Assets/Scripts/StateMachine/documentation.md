# State Machine

A state machine is an object that can be in one of multiple **states**. Each of these **states** performs a specific set of **actions**, and can **transition** to different **states** based on predefined **conditions**.

## Table Of Contents
- [Two-Parts implementation](#two-parts-implementation)
	- [ScriptableObjects](#scriptableobjects)
	- [Runtime Components](#runtime-components)
- [How-tos](#how-tos)
	- [Creating a State](#creating-a-state)
	- [Creating Action and Conditions](#creating-actions-and-conditions)
	- [Creating a Transition Table](#creating-a-transition-table)

## Two-Parts Implementation
This implementation is divided into two parts: **ScriptableObjects** and **Runtime** components.

### ScriptableObjects
The `ScriptableObject` part of the implementation is made as a designer-friendly, modular way of building state machines.

- **StateSO**: A `ScriptableObject` that can hold multiple `StateActionSO`. 
- **StateActionSO**: A `ScriptableObject` that wraps a **StateAction**.
- **StateConditionSO**: A `ScriptableObject` that wraps a **Condition**.
- **TransitionTableSO**: A `ScriptableObject` that defines the **Transitions** of a **StateMachine**.

### Runtime Components
The runtime part of the implementation has its name from being the one that contains the code that gets executed during the gameplay loop.

- **State**: A class that contains multiple `Action` and `Transition`. The `State` is responsible for routing the callbacks from the `StateMachine` to each `Action` and `Transition`. This class is only used internally.
- **StateAction**: A class that represents an action to perform. This is the base class all custom actions must inherit from.
- **StateTransition**: A class that contains a target `State`, and a set of `Condition` that must be evaluated to transition to it. Each `State` holds its own `Transitions` list. This class is only used internally.
- **Condition**: A class that represents a conditional statement. This is the base class all custom conditions must inherit from.
- **StateCondition**: A struct that holds a `Condition` and the expected result of the condition. This allows for reusability for when a `Condition` should sometimes evaluate to `true`, and sometimes to `false`. This struct is only used internally.
- **StateMachine**: A `MonoBehaviour` that can be attached to `GameObjects` that require the use of a state machine pattern. It takes in a `TransitionTableSO` and, during its `Awake` call, each `ActionSO`, `ConditionSO`, and `StateSO` in it is resolved into its runtime counterpart, and the `Transitions` lists are generated. In `Update`, `Actions` of the current `State` are performed and then `Conditions` of every `Transition` of the current `State` are checked. The first `Transition` that evaluates to `true` gets triggered.

## How-Tos
### Creating a State
To create a new `StateSO`, simply open the Assets menu and navigate to Create > State Machines > State. Name your new State and then you can start adding actions to it. Note that the order in which they are displayed corresponds to the order in which they are performed; the custom editor allows for easy sorting so you can organize them accordingly.
### Creating Actions and Conditions
To create a new `Action` or `Condition` script, in the Assets menu go to Create > State Machines > Action/Condition Script. This will create a new script from a template that will contain both the **SO** class and the **Runtime** class. You can modify both classes to your needs. Remember that after creating the script, you need to create at least one instance of the SO to use it in your state machine. The template already adds the Asset menu item for you, under Create > State Machines > Actions/Conditions > Your New Script.
### Creating a Transition Table
You can create a new Transition Table by opening the Asset menu > Create > State Machines > Transition Table.

From there, you can start adding transitions by using the **Add transition** button. Each transition needs a 'From State' and a 'To State'. 'From State' refers to the state the state machine should be in so that it can transition to the 'To State'. After setting them, you also need to add the conditions that must be met to make the transition happen. You can chain conditions with the And/Or option, and set whether the condition should be `true` or `false`, allowing for multiple scenarios in which the transition should take place. Once everything is set up, click on **Add transition** once again to confirm your changes.

The transition table is organized by 'From States'. This means that you'll see expandable groups with the names of the 'From States' as their headers, and expanding one will list all of the 'To States' it can transition to, along with their conditions. Transitions that take priority within a 'From State' should be sorted first, as they are checked sequentially and the first one that is met will trigger the transition. You can sort them with the up and down arrow buttons by the 'To State'. You can also remove a transition with the minus(-) button. Removing all the 'To State's from a 'From State' will remove the 'From State' from the table. You can also sort the 'From State's. However, only the first one is important to set up correctly as it will be the initial state of your state machine; subsequent ordering is purely preferential. 

Finally, you can edit your Transition Table by finding the asset in the Project window, or by navigating to ChopChop > State Machine > Transition Table Editor and selecting it from the list.
