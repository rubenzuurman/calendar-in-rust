# Readme

## Idea
The idea is to have a database of different types of events (similar to a calendar) which' history is completely known, so that the calendar can be rebuilt from the history. The history will be saved in the form of a tree.

The Calendar contains a list of CalendarEntry's retrieved from the history tree. I imagine at least the following types of events will be available (implemented following [this stackoverflow answer](https://stackoverflow.com/questions/70319364/rust-name-fields-in-enum)) (timestamp from chrono crate):<br />
&emsp;Event(id: u32, event_timestamp: i64, title: String, description: String)<br />
&emsp;EventOnLocation(id: u32, event_timestamp: i64, title: String, description: String, location: GeoLocation)<br />
&emsp;Reminder(id: u32, event_timestamp: i64, title: String, description: String)<br />
The Calendar has methods allowing for adding, removing, and updating events by id. These methods will add leaves to the tree and then update the calendar from the tree.

The tree will preferably be a class accepting any generic type implementing the Clone trait. The leaves of a tree are of type Leaf, which is an enum with four options:<br />
&emsp;Root(leaf_id: u32)<br />
&emsp;Add(leaf_id: u32, parent_leaf_id: u32, timestamp: i64, object: T)<br />
&emsp;Remove(leaf_id: u32, parent_leaf_id: u32, timestamp: i64, object_id: u32)<br />
&emsp;Update(leaf_id: u32, parent_leaf_id: u32, timestamp: i64, object_id: u32, updates: HashMap<String, CalendarEntryField\>)<br />
The CalendarEntryField is again an enum, containing the following options for now (more will be added when necessary for future CalendarEntry options): <br />
&emsp;Timestamp(i64)<br />
&emsp;Text(String)<br />
&emsp;Location(GeoLocation)<br />

GeoLocation is a class for storing geographical locations with the following fields:<br />
&emsp;latitude: f64<br />
&emsp;longitude: f64<br />
This is just a convenience class so I can store it into CalendarEntry::EventOnLocation and CalendarEntryField::Location.

The Calendar has a method called from_tree(), which builds the Calendar from the given Tree by sorting the tree leaves by timestamp and applying all leaves in chronological order starting from the first leaf that was added after the time the Calendar was last updated or created.

When a CalendarEntry is added, removed, or updated, a leaf is created and added to the history tree. The Calendar can then by updated by calling from_tree().

I also want the tree to be serializable.

## Classes and enums
Classes:<br />
&emsp;Calendar<br />
&emsp;HistoryTree<br />

enums:<br />
&emsp;CalendarEntry<br />
&emsp;CalendarEntryField<br />
&emsp;Leaf<br />
