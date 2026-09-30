Before data layer implementation, aswer the next questions:

As an application developer, you look at the real world (in which there are peo‐
ple, organizations, goods, actions, money flows, sensors, etc.) and model it in
terms of objects or data structures, and APIs that manipulate those data struc‐
tures. Those structures are often specific to your application.
2. When you want to store those data structures, you express them in terms of a
general-purpose data model, such as JSON or XML documents, tables in a rela‐
tional database, or a graph model.
3. The engineers who built your database software decided on a way of representing
that JSON/XML/relational/graph data in terms of bytes in memory, on disk, or
on a network. The representation may allow the data to be queried, searched,
manipulated, and processed in various ways.
4. On yet lower levels, hardware engineers have figured out how to represent bytes
in terms of electrical currents, pulses of light, magnetic fields, and more.



### Relational Model Versus Document Model

### The Birth of NoSQL
A need for greater scalability than relational databases can easily achieve, includ‐
ing very large datasets or very high write throughput
• A widespread preference for free and open source software over commercial
database products
• Specialized query operations that are not well supported by the relational model
• Frustration with the restrictiveness of relational schemas, and a desire for a more
dynamic and expressive data model


### The Object-Relational Mismatch
The discon‐
nect between the models is sometimes called an impedance mismatch

Object-relational mapping (ORM) frameworks like ActiveRecord and Hibernate
reduce the amount of boilerplate code

data structure like a résumé, which is mostly a self-contained document, a JSON is better. JSON has the appeal of
being much simpler than XML. Document-oriented databases like MongoDB [9],
RethinkDB [10], CouchDB [11], and Espresso [12] support this data model.

make marmeid schema example for: (Figure 2-2. One-to-many relationships forming a tree structure.)
JSON representation has better locality than the multi-table schema

### Many-to-One and Many-to-Many Relationships

benefits of the document oriented DB
• Consistent style and spelling across profiles
• Avoiding ambiguity (e.g., if there are several cities with the same name)
• Ease of updating—the name is stored in only one place, so it is easy to update
across the board if it ever needs to be changed (e.g., change of a city name due to
political events)
• Localization support—when the site is translated into other languages, the stand‐
ardized lists can be localized, so the region and industry can be displayed in the
viewer’s language
• Better search—e.g., a search for philanthropists in the state of Washington can
match this profile, because the list of regions can encode the fact that Seattle is in
Washington (which is not apparent from the string "Greater Seattle Area")

the ID can remain the same, even if the information it identifies
changes.

if that information is duplicated, all the redundant copies need to be
updated. 

write overheads, and risks inconsistencies (where some copies
of the information are updated but others aren’t)

### Normalization in document model.
Removing such duplication is the
key idea behind normalization in databases

Unfortunately, normalizing this data requires many-to-one relationships (many peo‐
ple live in one particular region, many people work in one particular industry), which
don’t fit nicely into the document model.

In document databases, joins are
not needed for one-to-many tree structures

If the database itself does not support joins, you have to emulate a join in application
code by making multiple queries to the database.

work of making the join is shifted
from the database to the application code


Draw marmeid schema with example


### Example of the Normalization in document model.

Organizations and schools as entities
In the previous description, organization (the company where the user worked)
and school_name (where they studied) are just strings. Perhaps they should be
references to entities instead? Then each organization, school, or university could
have its own web page (with logo, news feed, etc.); each résumé could link to the
organizations and schools that it mentions, and include their logos and other
information (see Figure 2-3 for an example from LinkedIn).
Recommendations
Say you want to add a new feature: one user can write a recommendation for
another user. The recommendation is shown on the résumé of the user who was
recommended, together with the name and photo of the user making the recom‐
mendation. If the recommender updates their photo, any recommendations they
have written need to reflect the new photo. Therefore, the recommendation
should have a reference to the author’s profile.

### Are Document Databases Repeating History?

TODO prvode example:
Developers
had to decide whether to duplicate (denormalize) data or to manually resolve refer‐
ences from one record to another. These problems of the 1960s and ’70s were very
much like the problems that developers are running into with document databases
today

### The network model

TODO - provide main concept explanation of the network model with example
TODO - prepare example for network model, is it database or concept? 
TODO - create STAR for explanation

make pints and marmeid diagram for explanation the next:

Conference on Data Systems Languages (CODASYL)
The CODASYL model was a generalization of the hierarchical model. In the tree structure of the hierarchical model, every record has exactly one parent;

For example, there could be one
record for the "Greater Seattle Area" region, and every user who lived in that
region could be linked to it. This allowed many-to-one and many-to-many relation‐
ships to be modeled.

links between records in the network model were not foreign keys, but more like
pointers in a programming language (while still being stored on disk) This was called an access path.

like the traversal of a linked list: start at
the head of the list, and look at one record at a time until you find the one you want.

many-to-many relationships, several different paths can lead to the
same record

### The relational model

TODO - draw mermeid diagram for explanation:
TODO - describe main points how it works 
TODO - describe pros and cons

relation (table) is simply a collection of tuples (rows), and that’s it.

There are no laby‐
rinthine nested structures, no complicated access paths to follow if you want to look
at the data. 

You can read any or all of the rows in a table, selecting those that match
an arbitrary condition. 

You can read a particular row by designating some columns as a key and matching on those.

You can insert a new row into any table without
worrying about foreign key relationships to and from other tables

query optimizer automatically decides which parts of the
query to execute in which order, and which indexes to use. Those choices are effec‐
tively the “access path,” 

### Relational Versus Document Databases Today

main arguments in favor of the document data model are schema flexibility

better performance due to locality, and that for some applications it is closer to the data
structures used by the application. 

The relational model counters by providing better
support for joins, and many-to-one and many-to-many relationships.

### Which data model leads to simpler application code?

data in your application has a document-like structure - probly a good idea to use a document model

relational technique of shredding—
splitting a document-like structure into multiple tables (like positions, education,
and contact_info, etc

The poor support for joins in document databases may or may not be a problem, depending on the application. For example many-to-many relationships may never
be needed in an analytics application that uses a document database

However, if your application does use many-to-many relationships, the document
model becomes less appealing

### Schema flexibility in the document model

Document databases are sometimes called schemaless

TODO - explain pros and cons:
doc db:
if (user && user.name && !user.first_name) {
// Documents written before Dec 8, 2013 don't have first_name
user.first_name = user.name.split(" ")[0];
}

relational db:
ALTER TABLE users ADD COLUMN first_name text;
UPDATE users SET first_name = split_part(name, ' ', 1); -- PostgreSQL
UPDATE users SET first_name = substring_index(name, ' ', 1); -- MySQL

Schema changes have a bad reputation of being slow and requiring downtime.... etc... 


TODO:
Continue - Page 63
Data locality for queries


