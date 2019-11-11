# LoadingBdTreesNotes
Some performance notes/tips when using Behavior Designer 1.6.3 Asynchronous Loading

I've finally got to the point in my game project where I need to start optimzing things. As I have been
I noticed that when I was enabling 10-20 enemies at a time during the game(think Monster Closets) the game 
was freezing for a few frames. I checked the profiler and when my Behavior Trees were being enabled they
were generating 20 mb of Garbage(GC) and causing a 460ms hit to the frame rate 0_0

I upgraded to Behavior Designer 1.6.3 as it has an Asynchronous loading of trees- I tried it out and got
some errors. As of right now it seems Async Loading throws errors on some Movement Pack tasks- here is how 
to fix those errors. The errors are caused by some initializion code in those tasks not being thread safe.
Anyway its a simple fix.

You have to change the following tasks- CanSeeObject, WithinDistance, Search

On CanSeeObject make line 24 look like this-
 public LayerMask ignoreLayerMask;
 
 line 49 like this-
      private int ignoreRaycastLayer;
 
 and add this on line 50 or 51-
      public override void OnStart()
        {
            base.OnStart();
            ignoreRaycastLayer = LayerMask.NameToLayer("Ignore Raycast");
        }

On WithinDistance make line 26 look like this-
      public LayerMask ignoreLayerMask;

Delete line 119

On Search make line 28 look like this-
        public LayerMask ignoreLayerMask;
        
That should do it!

Here's a chart showing the performance improvements in my game loading a group of enemies during gameplay-

![alt text](https://i.imgur.com/44QcJIZ.png)
