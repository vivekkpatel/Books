# AutoCAD Platform Customization: VBA by Lee Ambrosius

### Autocad Main Objects

### **Application**

Returns the Acad Application object that represents the current AutoCAD session.

### Document

Returns the AcadDocument object that represents the drawing in which
the object is stored.

### Handle

Returns a string that represents a unique hexadecimal value that
differentiates one object from another in a drawing; think of it along the
lines of a database index. An object’s handle persists between drawing
sessions. A handle, while unique to a drawing, can be assigned to another
object in a different drawing.

### ObjectID

Returns a unique integer that diff Derentiates one object from another in a
drawing; think of it along the lines of a database index. Unlike a handle,
the object ID of an object might be different each time a drawing is loaded
into memory.

### ObjectID32

Same as the ObjectID property, but must be used on 64-bit releases of
Windows. Th is property is only valid with AutoCAD 2009 through 2014.
Use the ObjectID property for earlier releases and AutoCAD 2015.

### ObjectName

Returns a string that represents the object’s internal classname.
Th is value can be used to distinguish one object type from
another as part of a conditional statement.

### OwnerID

Returns the object ID of the object’s parent. For example, the parent of a
line might be model space or a named layout whereas the text style symbol
table is the parent of a text style.

### OwnerID32

Same as the OwnerID property, but must be used on 64-bit releases of
Windows. Th is property is only valid with AutoCAD 2009 through 2014.
Use the OwnerID property for earlier releases and AutoCAD 2015.

# Properties related to the AcadEntity object

**AcadEntity**
All graphical objects in a drawing are represented by the AcadEntity object. The AcadEntity
object inherits the properties and methods of the AcadObject object and adds additional properties and methods that all graphical objects have in common. For example, all graphical objects can be assigned a layer and moved in the drawing. The Layer property of the AcadEntity object is used to specify the layer in which an object is placed, and the Move method is used to relocate

an object in the drawing. Objects based on the AcadEntity object can be added to model space,a named layout, or a block defi nition.

**EntityTransparency**

Specifies the transparency for an object.

**Hyperlinks**
Returns the AcadHyperlinks collection object assigned to an object.

**Layer**
Specifies the layer for an object.

**Linetype**
Specifies the linetype for an object.

**LinetypeScale**
Specifies the linetype scale for an object.

**Lineweight**
Specifies the lineweight for an object.

**PlotStyleName**
Specifies the name of the plot style for an object.

**TrueColor**
Specifies the color assigned to an object.

**Visible**
Specifies the visibility for an object.

Methods related to the AcadEntity object

**ArrayPolar**
Creates a polar array from an object.

**ArrayRectangular**
Creates a rectangular array from an object.

**Copy**
Duplicates an object.

**GetBoundingBox**
Returns an array of doubles that represents the lower and upper points of an object’s extents.

**IntersectWith**
Returns an array of doubles that represents the intersection points between two objects.

**Mirror**
Mirrors an object along a vector.

**Move**
Moves an object.

**Rotate**
Rotates an object around a base point.

**ScaleEntity**
Uniformly increases or decreases the size of an object.

**Update**
Instructs AutoCAD to recalculate the display of an object; similar to the regen command but it only affects the object in which the method is executed.

### Working with Polylines

**PolyLine**
Legacy polylines were available in AutoCAD R13 and earlier releases, and they are still available in AutoCAD R14 and later releases. This type of polyline object supports 3D coordinate values, but it uses more memory and increase the size of a drawing fi le.

**Lightweight Polyline**
Lightweight polylines, or LWPolylines, were first introduced in AutoCAD R14. They are more efficient in memory and require less space in a drawing fi le. Lightweight polylines support only 2D coordinate values.

**Tip**
You use the AddVertex or AppendVertex method to add a new vertex to a polyline, but it isn’t exactly obvious how you might remove a vertex. To remove a vertex from a polyline, use the Coordinates property to get the vertices of the polyline. Th en create a new vertices list of the points you want to keep and assign the new vertices list to the Coordinates property.

**Tip**
Dimensions created with the AutoCAD Object library are not associative; the dimension
isn’t updated if the objects that the dimension measures are changed. If you want to create
associative dimensions, consider using the SendCommand or PostCommand method with the
appropriate command sequence.

<aside>
💡 Vbaman to load vba manager

</aside>