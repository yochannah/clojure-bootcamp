- [PREVIOUS: requirements](intro-to-clojure.md)
- [NEXT: TBD](re-frame-and-reagent.md)

# Some boring architecture stuff

BlueGenes is built on [re-frame](https://github.com/Day8/re-frame), which provides event handling and data management for [Reagent](http://reagent-project.github.io/) which is in itself a Clojurescript wrapper of the popular Javascript framework [React](https://reactjs.org/). From here on we'll mostly be talking about re-frame, and occasionally reagent.

## re-frame has an in-browser-memory database that knows everything.

This is known as [App DB](https://github.com/Day8/re-frame/blob/master/docs/ApplicationState.md). It's basically just a giant map, like the maps we covered [last chapter](intro-to-clojure.md#map)).

What's neat about Re-frame, though, is that when you access properties from the app-db using **subscriptions**, your page will automatically be updated if app db changes. Reading the results of a description is also known as _dereferencing_ the subscription.

When something happens, you can also dispatch **events** - e.g. if a user clicks on a button, you might dispatch a page navigation event.  

Re-frame also has a concept of **effects** - that is, things that will happen in response to events, but that might not be under the control of app DB.

### Example Scenario:

1. Imagine that you're viewing the results of a query in an im-table. App-db might look like this:
```clojure
{
  :notification-bar-contents nil
  :current-mine-url "http://www.kittenmine.com/kittenmine"
  ;;the user doesn't have any lists yet
  :user-lists []
}
```
2. On the page, there's a button enticing the user to save a list. When the user clicks this button, an event is dispatched - let's say it's called `::query/save-to-list`. Several things happen in response to this event being triggered:
    - the code for `::query/save-to-list` updates app-db, by setting the value of `:notification-bar-contents` to true. Thanks to a well-placed subscription, the notification is automatically displayed on the page. `::query/save-to-list` also dispatches an _effect_, as well.

    Here's what app-db looks like right now:

    ```clojure
    {
      ;;We've given the user a status update!
      :notification-bar-contents "Saving..."
      :current-mine-url "http://www.kittenmine.com/kittenmine"
      ;;we won't add the user's list name into this vector until we
      ;;get a success message back from kittenmine.
      :user-lists []
    }
    ```

    - The first effect is triggered to make the http request to kittenmine to save the list to kittenmine's db. Let's call the effect `::query/dispatch-save-list-to-intermine`.
    - once a "success" response comes back from kittenmine, a second "success" effect `::query/dispatch-save-list-to-intermine-sucess` is triggered - in this case, we save the list name to the app-db so it can load quickly on the MyMine page. We didn't want to save it as part of the first effect because the save list operation could have failed on kittenmine's side, perhaps because there was already a list with the same name, or because the connection was interrupted. We also set `:notification-bar-contents` to display a success message, since we're done, and the user will automatically see the notification bar text change, since it's accessed on the page via a subscription. This is how app-db looks now:
```clojure
{
  ;;we've updated that status bar
  :notification-bar-contents "List successfully saved!"
  :current-mine-url "http://www.kittenmine.com/kittenmine"
  ;;look, the user has a personal list now!
  :user-lists ["List of genes that make cats extra fuzzy"]
}
```

## Confused yet?

Try cloning https://github.com/yochannah/bootcamp-demo or just browse the files. The readme has instructions for running the project - give it a try.

When you're browsing the code, the most interesting bits are probably in [/src/cljs/bootcamp_demo/](https://github.com/yochannah/bootcamp-demo/tree/master/src/cljs/bootcamp_demo) - looks at the view, events, and subs files to see how the lifecycle works, and then trying running the project.

- See that little re-frisk arrow at the bottom of the page? That's a developer extension that allows you to browse app-db. Try updating the number field and see how app-db changes.
- Try tweaking the files or functions to do different things. Figwheel will auto-reload every time you save a file, and will tell you in the browser if the code didn't compile.

Consider reviewing [the re-frame code walkthrough](https://github.com/Day8/re-frame/blob/master/docs/CodeWalkthrough.md) for a more in depth explanation of re-frame's lifecycle.



## Additional resources:

These will cover re-frame, reagent, and react more generally and may touch on things we haven't really looked at or don't use much in BlueGenes:

- [re-frame](https://github.com/Day8/re-frame) re-frame - there are lots of docs but they are often chaotic and jump about or don't reference the current version. It adds an event and data management layer on top of Reagent.  
- [Reagent](http://reagent-project.github.io/) the clojure wrapper for React
- [React](https://reactjs.org/) You probably won't learn much useful here as Reagent is too far abstracted inlanguage and behavior, but React *is* the underpinning of BlueGenes.  
- [React Lifecycle](http://busypeoples.github.io/post/react-component-lifecycle/) - think twice or thrice if feel you need to access the react lifecycle in re-frame. You usually don't.

- [PREVIOUS: requirements](intro-to-clojure.md)
- [NEXT: TBD](re-frame-and-reagent.md)
