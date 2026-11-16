# Reflection

## 1. What my functions do and their time complexity

1. **TODO 1:** The `Scene` constructor saves the scene's data and creates the correct number of choice slots. I also set every slot to `nullptr` so they start empty and are safe to delete later. Since each scene can only have up to three choices, this is O(1).

2. **TODO 2:** The `Scene` destructor deletes all of the child scenes, the choices array, and the scene's data. Since it can delete every scene underneath it, its worst-case time complexity is O(n).

3. **TODO 3:** `isEnding` checks if the scene has zero choices. If it does, then it is an ending. This is O(1).

4. **TODO 4:** `getChoice` checks whether the index is valid and then returns the scene at that position. This takes O(1) time.

5. **TODO 5:** `setChoice` checks the index and stores the next scene in that slot. This is also O(1).

6. **TODO 6:** `countScenes` counts the current scene and then recursively counts all of its children. Since every scene is visited once, it takes O(n).

7. **TODO 7:** `countEndings` checks every scene and adds one whenever it finds a scene with no choices. It also takes O(n).

8. **TODO 8:** `height` finds the longest path by checking the height of every child and keeping the largest one. It visits every scene once, so it is O(n).

9. **TODO 9:** `clear` deletes the root scene and sets the root pointer to `nullptr`. The Scene destructor handles the rest of the tree, making the total complexity O(n).

10. **TODO 10:** `load` opens the story file, reads the starting HP, builds the story tree, and stores the root in the tree. It reads the story once, so it is O(n).

11. **TODO 11:** `readScene` reads one scene and then recursively reads all of the scenes underneath it. It also checks that the number of choices is between 0 and 3. Since each scene is processed once, it is O(n).

12. **TODO 12:** `askChoice` reads the player's input and checks whether it is a valid number in the correct range. Since there can only be three choices, it is O(1) with respect to the size of the story.

13. **TODO 13:** `play` follows one path through the story and updates the player's HP, loot, and inventory. A path can include up to n scenes. Since searching the inventory can also take up to n time, the worst-case complexity is O(n²).

14. **TODO 14:** `printStats` prints the number of scenes, endings, and the height of the story. Each tree calculation is O(n), so the whole function is still O(n).

15. **TODO 15:** `printSpoilers` starts the recursive function that prints every ending. Since the whole tree is visited once, it is O(n).

16. **TODO 16:** `printEndings` goes through each choice recursively and prints the path when it reaches an ending. It takes O(n).

17. **TODO 17:** `greedyRun` follows one path and chooses the next scene with the most immediate loot. Since it only travels down one path, it takes O(n) in the worst case.

18. **TODO 18:** `bestLoot` checks every possible path and keeps the best path that does not kill the player. Because the story is a tree and each scene is visited once, it takes O(n).

19. **TODO 19:** `optimalRun` calls `bestLoot` from the root and prints the result. Its complexity is O(n), since it depends on `bestLoot`.

## 2. Why each scene allocates exactly its number of choices

Each scene only creates as many choice slots as it actually needs. For example, a scene with two choices only creates two slots. Using a fixed array of three pointers would waste space and would not follow the exact-size allocation requirement. An ending has zero choices, so its `choices_` pointer stays as `nullptr` and no array is created.

## 3. How the Scene destructor works

The destructor first deletes each child scene stored in the choices array. Those child destructors then delete their own children, so the whole tree is eventually cleaned up. After deleting the children, it deletes the choices array and then the scene's data. If the choices array were deleted first, I would lose access to the child pointers and they would leak from memory.

## 4. Greedy versus optimal on my Pokémon story

The greedy advisor takes path **1-1**. It first chooses the strange Poke Mart because it gives 8 immediate loot, which looks better than the other starting choices. At the Poke Mart, it presses the red button because that choice gives 4 more loot instead of the 1 loot from asking Rotom for help. The problem is that the red button is a trap and causes enough damage to kill Eevee. The greedy path collects 12 loot but does not reach an ending.

The optimal advisor takes path **2-1-1-1-1** and collects 14 loot while surviving. This path follows Pikachu's sparks, enters the abandoned gym, crosses the electric arena, climbs the championship stairs, and rings the League bell. I made the Poke Mart look like the best option at first, but made the red button dangerous so that the greedy strategy would fail. This shows that the choice with the biggest reward right away is not always the best overall choice.

## 5. Why bestLoot is O(n)

`bestLoot` checks every possible child, but the story is already arranged as a tree. Each scene only has one parent, so the function visits each scene once instead of recalculating the same scene over and over. It compares the results from each child and keeps the best surviving result. Because each scene is processed once, the function is O(n) instead of exponential.

## 6. List methods used by the game

The game uses `search` to check if the player already has an item, `addFront` to add a new item to the inventory, `size` to count the items, and `print` to display the inventory at the end. The list destructor uses `deleteFront` to clean up the items. One `search` takes O(k) time, where k is the number of items currently in the inventory, because it may need to check every item in the list.