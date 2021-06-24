---
layout: default
title: Drawing event data
parent: REST Basics
nav_order: 8
has_children: true
---

## Drawing event data

## Browsing and viewing events using TRestBrowser

Events in REST data files are managed by TRestRun, whose graphical interface, in turn, is shown by
by TRestBrowser. This class shows a TBrowser window during initialization. In the window there is a 
canvas showing the current event, and a control panel to switch between events. 

In restRoot prompt, by using TRestBrowser, one can easily get accsess to the file's events, and don't
need to manually instantiate a TRestEvent object and set the tree's branch address. He just needs to type:  
`restRoot abc.root`  
`TRestBrowser a`  
`TRestxxxEvent*eve=(TRestxxxEvent*)a.GetInputEvent()`,  
and will be free to operate this event.

By default TRestBrowser extracts the last event in file, and draws it in the canvas by using the viewer
class TRestGenericEventViewer. This viewer just calls the default method TRestEvent::Draw(). Other viewers
like TRestHitsEventViewer or TRestGeant4EventViewer are also available. Some pre-defined bash alias and ROOT 
scripts can be used to draw these events in differently. In bash, we can directly start a event viewer 
window with commands: "restViewEvents abc.root", "restManager ViewHitsEvents hits.root". In restRoot 
prompt, we can call the function: "REST_ViewEvents("abc.root")" to start the event viewer.

Here for example, we use the generated file in [example](process-a-raw-data-file), and call the command 
`restViewEvents abc.root`. The last event is TRestRawSignalEvent type in this file, and a TRestBrowser
window will show up drawing the waveforms. In the command line it will print observable values.

![alt](assets/images/restViewEvents.png)

In the TRestBrowser window, on the right side there is a combined plot of the event, which contains 
several individual signal waveforms. In the left side we have a control panel. The arrow buttoms and the 
text box in upper area helps to switch next/previous/specific event. The browser also supports plot 
options. If we click on the lower buttoms, for TRestRawSignalEvent it will plot next/previous/specific
signals inside the current event.

Some viewer processes are also available in REST. The user can have a view of the events during 
the process. All the viewer processes are single thread only, and TRestProcessRunner will automatically
roll back to single thread mode with a viewer process in process chain. 
