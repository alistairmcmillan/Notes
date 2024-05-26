# How to change the y coord of multiple objects at once

Hold down ALT (Win) or Option (Mac) before clicking into the coord input box

# How to stabilise one axis of a video

1. Click Editor button in top-left and change to "Movie Clip Editor"
2. Open video in editor
3. Click "Set Scene Frames" to set timeline to same length as video
4. Set Correlation to 0.9 in Tracking Settings Extra
5. Hold CMD and left-click to reate first tracking point
6. Scale (S key) to have identifiable feature in tracking point
7. Click "Clip Display" in top-right and tick "Search" to display search box around tracking point
8. Let Blender track the feature through the video
    1. CMD and T to start automatic tracking forward through frames
    2. SHIFT, CMD and T to track automatically background through frames
    3. OPTION and right key to track forward one frame
    4. OPTION and left key to track forward one frame
9. If the point you are tracking goes outside frame hit G key and G key again to create new offset tracking point
10. Once you have the full path tracking, select the "Compositing" workspace at the top of the window
11. Tick "Use Nodes"
12. Select and delete the default "Render Layers" node
13. Add "Movie Clips" node from Input sub-menu, and select the clip you've just been working with at the bottom of the node
14. Add "Transform" node from Distort sub-menu
15. Add "Viewer" node form Output sub-menu
16. Connect image between all nodes
17. Click "Fit" on View tab of sidebar
18. To eliminate X axis motion connect "Offset X" from Movie Clips node to Transform node
19. Set Scale of Transform node to 1.1
20. Set Render Engine of Render Properties to Eevee
21. Set Frame Rate of Output Properties to same frame rate as input video
22. Set Resolution X and Resolution Y to same width and height as input video
23. Pick an output directory
24. Set File Format to PNG
25. Set Compression to 0%
26. Pick Render Animation from Render menu
