[PREVIOUS: requirements](../requirements.md)
[NEXT: re-frame and reagent](re-frame-and-reagent.md)

# Some boring architecture stuff

BlueGenes is built on [re-frame](https://github.com/Day8/re-frame), which provides event handling and data management for [Reagent](http://reagent-project.github.io/) which is in itself a Clojurescript wrapper of the popular Javascript framework [React](https://reactjs.org/). From here on we'll mostly be talking about re-frame, and occasionally reagent.

1. [App DB](https://github.com/Day8/re-frame/blob/master/docs/ApplicationState.md) - where everything you know is stored.
2. Events, effects, and subscriptions. Start by reviewing [the re-frame code walkthrough](https://github.com/Day8/re-frame/blob/master/docs/CodeWalkthrough.md)



## Additional resources:

These will cover re-frame, reagent, and react more generally and may touch on things we haven't really looked at or don't use much in BlueGenes:

- [re-frame](https://github.com/Day8/re-frame) re-frame - there are lots of docs but they are often chaotic and jump about or don't reference the current version. It adds an event and data management layer on top of Reagent.  
- [Reagent](http://reagent-project.github.io/) the clojure wrapper for React
- [React](https://reactjs.org/) You probably won't learn much useful here as Reagent is too far abstracted inlanguage and behavior, but React *is* the underpinning of BlueGenes.  
- [React Lifecycle](http://busypeoples.github.io/post/react-component-lifecycle/) - think twice or thrice if feel you need to access the react lifecycle in re-frame. You usually don't.

[PREVIOUS: requirements](../requirements.md)
[NEXT: re-frame and reagent](re-frame-and-reagent.md)
