# Start Programming in NET for AutoCAD by Anton Huiznga

### Code for object selection

```csharp
[CommandMethod(nameof(ObjectSelection))]
        public void ObjectSelection()
        {
            Editor editor = Application.DocumentManager.MdiActiveDocument.Editor;

            PromptEntityResult per = editor.GetEntity("Select Object");

            //Check if user select soemthing or press esc to cancel
            if (per.Status == PromptStatus.OK)
            {
                //means something is selected
                editor.WriteMessage("An Object is selected");
            }
            else
            {
                //Means escape is pressed to cancel selection
                editor.WriteMessage("You fool, you have to do it again");
            }
        }
```

### Code to select multiple lines

```csharp
// Method for multi Select
        [CommandMethod(nameof(MultipleObjectSelection))]
        public void MultipleObjectSelection()
        {
            Editor editor = Application.DocumentManager.MdiActiveDocument.Editor;

            PromptStatus status = PromptStatus.OK;
            while (status==PromptStatus.OK)
            {
                PromptEntityResult prompt = editor.GetEntity("Select Object");
                editor.WriteMessage("An object is selected");
                status = prompt.Status;
            }
        }
```

### Reading Entity properties

In AutoCAD, Entities are objects of the type Entity. They are also objects of the type Polyline, Line, Circle or whatever other types there are. Confusing perhaps?

A polyline, of course, is an object of the type Polyline. A circle is an object of the type Circle. But both are at a lower level also an object of the type Curve and at an even lower level an object of the type Entity. That's because they share certain properties.

Each object can have properties. With cars it is simple: color, number of doors, you name it. For a polyline, the number of kink points, the layer, and the color are examples of properties.

To read these properties, you will first need to create an object of the type corresponding to this object. Because the function GetEntity() only returns an ObjectId of the selected Entity and not the Entity itself.

```csharp
[CommandMethod(nameof(GetObjectProperties))]
        public void GetObjectProperties()
        {
            Editor editor = Application.DocumentManager.MdiActiveDocument.Editor;

            PromptStatus status = PromptStatus.OK;

            int totalCount = 0;
            while (status==PromptStatus.OK)
            {
                PromptEntityResult result = editor.GetEntity("\nSelect Object");
                status= result.Status;
                if (result.ObjectId!=null)
                {
                    totalCount++;
                    editor.WriteMessage("\nObject Number:" + totalCount.ToString());

                    //Code to Get Document Properties
                    using (Transaction transaction=Application.DocumentManager.MdiActiveDocument.TransactionManager.StartTransaction())
                    {
                        Entity entity = transaction.GetObject(result.ObjectId,OpenMode.ForRead) as Entity;

                        editor.WriteMessage("\nLayer : "+entity.Layer);
                        editor.WriteMessage("\nObject Type:"+ entity.GetType().ToString());
                        editor.WriteMessage("\n");

                        transaction.Commit();
                    }
                }   
            }

        }
```

Later in this book, ObjectIds will be explained in more detail. For now, it is enough to know that an ObjectId is a database reference to an object in the drawing.

> To use an ObjectId to find the Entity, a Transaction is needed. You see, the drawing is a database, a big bin full of Entities, but also full of tables like Layers, TextStyles, and so on.
> 

To retrieve something from that database, you create an object tr of the type Transaction. When creating it, you pass in the value returned by the StartTransaction() function of the active drawing (MdiActiveDocument)

The object tr has a function to retrieve an Entity object using an ObjectId. Pay close attention to the line:

```csharp
Entity entity = transaction.GetObject(per.ObjectId, OpenMode.ForRead) as Entity;
```

Here you create an object ent of the type Entity and you give it the value that comes from the function GetObject(). Since an Entity object does not come out of this directly, you convert it to an Entity by the addition 'as Entity'. This is called casting. So what comes out of the GetObject() function is cast or mold into an Entity. Casting is common in the programming world.

### Transactions

You'll probably wonder why you need a Transaction. Transactions are incredibly important as soon as you work with databases. And a drawing is a database.

In that drawing database are tables with layer names, tables with text styles, tables with block definitions and tables with Entities. To communicate with that database you need a Transaction. It can fetch objects from the database for you if you only have an ObjectId and can put objects into the database that you have created yourself. Or modified.

transaction.Commit() is an important function of the Transaction. It actually applies all changes and additions to the drawing database. As long as no Commit() is executed, the drawing database remains unchanged.

this is beneficial because otherwise it could corrupt the drawing. Suppose your function changes all kinds of things in the database and halfway through the function it stops working because of an error. The database is then filled with incorrect objects. A reference to a layer name that does not yet exist in the drawing for example.

Therefore all changes within the Transaction happen somewhere in memory and are only finally applied to the drawing database with the function Commit(). If you forget to call this function then nothing has happened in the drawing.

### Object Ids and Handles

ObjectIds are references to database objects. Every object in the drawing database has an ObjectId as identification. If you have ever called up a LIST of an object in AutoCAD, then you have probably seen the term Handle pass by. Every Entity in AutoCAD has a Handle. This is a hexadecimal number like 14F or 3A7. But a drawing database contains more than just objects with Handles. A layer name does not have a Handle. Therefore, although every object in the database contains an ObjectId, not every object contains a Handle. An ObjectId is a very large number like 2152212828304.

When opening a drawing, AutoCAD will assign an ObjectId to each object in the database. This is not always the same number. Handles, however, always remain the same. If you open the same drawing tomorrow as you did today and there is a Line in it, then tomorrow that Line will have the same Handle as today, but not the same ObjectId.

As long as the drawing is open, this of course remains the same. And that's why you can refer to an ObjectId in your code just fine if you need a specific object. Many functions like GetEntity() will return the ObjectId of the Entity. You then use this ObjectId in your code to refer to that specific object.

In this case, the code always goes well because you only pass ObjectIds that are of the type Entity. But if you pass an ObjectId of a Layer or of a text style? Those can't just be converted to an Entity. AutoCAD will then show a Fatal Error as soon as you try to convert the ObjectId to an Entity object.

Another error that can occur is when you query an object that has been deleted. For example, if you delete a Layer in AutoCAD, it secretly continues to exist but gets a check mark in the drawing database for the IsErased property

This has two useful consequences: an UNDO is easily realized with this and the speed of work is not hindered. Only when saving the drawing are all objects to be deleted actually removed from the database.

If this were done immediately then each operation would take as long as saving and reopening the drawing. Large drawings take as much as tens of seconds to open.

You can avoid this kind of error by checking if an ObjectId is of a certain type, if the object has been deleted or by error interception.

### Layer creation function

```csharp
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.Geometry;
public static void CreateLayer(string name)
{
Database db = Application.DocumentManager.MdiActiveDocument.Database;
using (Transaction tr = db.TransactionManager. StartTransaction())
{
LayerTable lt = (LayerTable)tr.GetObject(db.LayerTableId, OpenMode.ForRead);
if (lt.Has(name) == false)
{
lt.UpgradeOpen();
LayerTableRecord ltr = new LayerTableRecord();
ltr.Name = name;
lt.Add(ltr);
tr.AddNewlyCreatedDBObject(ltr, true);
}
else
{
// The layer can exist but IsErased
LayerTableRecord ltr = (LayerTableRecord)tr.GetObject(lt[name],
OpenMode.ForRead);
if (ltr.IsErased == true)
{
lt.UpgradeOpen();
ltr = new LayerTableRecord();
ltr.Name = name;
lt.Add(ltr);
tr.AddNewlyCreatedDBObject(ltr, true);
}
}
tr.Commit();
}
}
```

Let this function settle in your mind for a while. It's a complex function with a lot of new stuff. Of course, you start again with a using for the Transaction. Then you start by creating an object of the type LayerTable. By now you know that the drawing is a database. The database contains several tables. For block definitions you have a table, for line type styles you have a table and for layer names you have a table, for everything you have a table. The table for layer names is of the type LayerTable (nice and simple) and you can cast such an object from the ObjectId that the database keeps for this table. You can see that in the line as a parameter of the function GetObject() where you refer to db.LayerTableId.

This object you have named lt. As you can see, this book uses simple object names. Usually an abbreviation or initial of the object type. You may name these as you like, as long as you remember what you used and the name is not a reserved word. So you may also use the following

```csharp
LayerTable layertableobject = (LayerTable)tr.GetObject(db.LayerTableId, OpenMode.ForRead);
```

The next line checks whether the layer name already exists. The object lt of the object type LayerTable has a nice function called Has(). This means something like "Does the LayerTable have the following layer name, yes or no". This function returns true or false.

Now you should be aware that AutoCAD does not immediately delete objects when you discard them from the drawing but marks them in the database as IsErased. Only when saving to the database will these objects disappear into the big void. But as long as a drawing is open, you create a layer and then delete it, this Property will return true if you want to know if the layer is present. Because you will also get back layers that are marked as deleted, that's why you need to do an extra check just to be sure.

The code to create the layer is complex. First, you call the UpgradeOpen() function on the LayerTable. This is necessary because the LayerTable is opened as ReadOnly. You can also skip this line but then you should have created the object lt with the OpenMode.ForWrite parameter. It doesn't matter whether you use one or the other method, probably only speed will be an issue.

Next you create a LayerTableRecord object with the name ltr. A LayerTable contains LayerTableRecords, one for each layer. You give this LayerTableRecord the desired name (and possibly color, line type, transparency and other layer options) and then you add it to the LayerTable.

You also add it to the Transaction with the line:

```csharp
tr.AddNewlyCreatedDBObject(ltr, true);
```

This seems duplicative. But this way the Transaction knows which objects have been added to the database. Earlier I explained that changes to the database are not applied until you call the Commit() function. If you forget this line, nothing happens in the drawing at all.

### Save data in drawing

**Overview**

There are several ways to store data. One of the oldest data facilities is called XData. You can store up to 16K of data on objects. The data can be easily read by Lisp, making it quite powerful. AutoCAD internal functions also sometimes store data as XData, such as hyperlinks. With the XDLIST command, you can query which data has been stored:

```csharp
*Registered Application Name: PE_URL
* Code 1000, ASCII string: https://blog.huiz.net
* Code 1002, Starting or ending brace: {
* Code 1000, ASCII string: Blog
* Code 1002, Starting or ending brace: {
* Code 1071, 32-bit signed long integer: 1
* Code 1002, Starting or ending brace: }
* Code 1002, Starting or ending brace: }
Object has 16311 bytes of Xdata space available
```

Because 16K of data is very little (or at least not always enough), another technique has been devised, namely XRecords. With this technique you can link unlimited amounts of data to objects.

If you have AutoCAD Map 3D or Civil 3D you can also store data as Object Data or Property Sets, but these are not recognizable in AutoCAD. If you want something generic, which works in AutoCAD, Civil 3D and AutoCAD Map 3D, then choose XRecords.

### Read and Write data Functions

```csharp
private static void SetXRecordInObject(ObjectId id, string key, ResultBuffer rb)
        {
            try
            {
                using (DocumentLock acLckDoc =
                Application.DocumentManager.MdiActiveDocument.LockDocument())
                {
                    Database db = Application.DocumentManager.MdiActiveDocument.Database;
                    using (Transaction tr = db.TransactionManager.StartTransaction())
                    {
                        using (Entity ent = tr.GetObject(id, OpenMode.ForWrite) as Entity)
                        {
                            if (ent != null)
                            {
                                if (ent.ExtensionDictionary == ObjectId.Null)
                                {
                                    ent.CreateExtensionDictionary();
                                }
                                using (DBDictionary xDict =
                                (DBDictionary)tr.GetObject(ent.ExtensionDictionary, OpenMode.ForWrite))
                                {
                                    using (Xrecord xRec = new Xrecord())
                                    {
                                        xRec.Data = rb;
                                        try
                                        {
                                            xDict.Remove(key);
                                        }
                                        catch (System.Exception) { }
                                        xDict.SetAt(key, xRec);
                                        tr.AddNewlyCreatedDBObject(xRec, true);
                                    }
                                }
                            }
                        }
                        tr.Commit();
                    }
                }
            }
            catch (System.Exception ex)
            {
                System.Console.WriteLine(ex.Message);
            }
        }
```

```csharp
private static List<object> GetXRecordFromObject(ObjectId id, string key)
        {
            List<object> returnValue = new List<object>();
            try
            {
                using (DocumentLock acLckDoc =
                Application.DocumentManager.MdiActiveDocument.LockDocument())
                {
                    Database db = Application.DocumentManager.MdiActiveDocument.Database;
                    using (Transaction tr = db.TransactionManager.StartTransaction())
                    {
                        using (Entity ent = tr.GetObject(id, OpenMode.ForRead, false) as Entity)
                        {
                            if (ent.ExtensionDictionary == ObjectId.Null) { return returnValue; }
                            try
                            {
                                using (DBDictionary xDict =
                                (DBDictionary)tr.GetObject(ent.ExtensionDictionary, OpenMode.ForRead, false))
                                {
                                    using (Xrecord xRec = (Xrecord)tr.GetObject(xDict.GetAt(key),
                                    OpenMode.ForRead, false))
                                    {
                                        TypedValue[] xRecData = xRec.Data.AsArray();
                                        foreach (TypedValue tv in xRecData) { returnValue.Add(tv.Value); }
                                    }
                                }
                            }
                            catch (System.Exception) { }
                        }
                        tr.Commit();
                    }
                }
            }
            catch (System.Exception ex)
            {
                System.Console.WriteLine(ex.Message);
            }
            return returnValue;
        }
```

Again, it's a lot of text with a chance of mistakes when retyping, so pay attention. These two functions are private and therefore only available within the class. They are generic functions to store different types of data which you can see by the use of object. An object of the type object can be anything, a number, a text, a reference to another object, you name it. Now to store very specific text (name of buyer/owner) or numbers (parcel number) additional functions are needed. Add these two functions to the same class:

```csharp
				/// <summary>
        /// Save a textstring in the ExtensionDictionary of an object
        /// </summary>
        public static void SetObjXRecordText(ObjectId id, string key, string value)
        {
            using (ResultBuffer resBuf = new ResultBuffer(new TypedValue((int)DxfCode.Text, value)))
            {
                SetXRecordInObject(id, key, resBuf);
            }
        }

        /// <summary>
        /// Save an integer in the ExtensionDictionary of an object
        /// </summary>
        public static void SetObjXRecordInt32(ObjectId id, string key, int value)
        {
            using (ResultBuffer resBuf = new ResultBuffer(new TypedValue((int)DxfCode.Int32, value)))
            {
                SetXRecordInObject(id, key, resBuf);
            }
        }
```

You can see that both functions call the private function SetXRecordInObject(). So it saves a lot of typing if you had wanted to include the code from that function in both of these functions. Here you see how easy it is to pull equal code separately into an additional function that is then shared.

 The same goes for the code to retrieve data. Again, you need functions that retrieve text or numbers from an object. Add these functions as well:

```csharp
				/// <summary>
        /// Read a string from the ExtensionDictionary of an object
        /// </summary>
        public static string GetObjXRecordText(ObjectId id, string key)
        {
            string returnValue = string.Empty;
            List<object> records = GetXRecordFromObject(id, key);
            if (records.Count > 0) { returnValue = Convert.ToString(records[records.Count - 1]); }
            return returnValue;
        }

        /// <summary>
        /// Read an integer from the ExtensionDictionary of an object
        /// </summary>
        public static int GetObjXRecordInt32(ObjectId id, string key)
        {
            int returnValue = -1;
            List<object> records = GetXRecordFromObject(id, key);
            if (records.Count > 0) { returnValue = Convert.ToInt32(records[records.Count - 1]); }
            return returnValue;
        }
```

Because the generic function GetXRecordFromObject() can return a list of values, more than one, because more were added or because the previous one was removed and added again, the last value from the list is returned as the value we want. This is done with the code: records[records. Countt - 1];

The index value of the record you want to retrieve is normally placed between square brackets. To retrieve the last one, use the Count minus 1 property. Since lists are zero-based indexed, a list of ten records will consist of record[0], record[1], through record[9]. Because the Count property returns the value 10, you therefore call the last record with Count-1.