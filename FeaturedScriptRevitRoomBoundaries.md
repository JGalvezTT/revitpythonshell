
```
'''
revitRoomBoundaries.py

displays a list of all elements bounding a room.
'''
import traceback
import clr
clr.AddReference('RevitAPI')
from Autodesk.Revit.DB.Architecture import Room
from Autodesk.Revit.DB import Wall

def main():
    try:
        sel = __revit__.ActiveUIDocument.Selection.Elements
        rooms = [e for e in sel if type(e) is Room]
        for room in rooms:
            for bsa in room.Boundary:
                for bs in bsa:
                    if type(bs.Element) is Wall:
                        print "Wall:", bs.Element.Id, bs.Element.Name, bs.Element.Level.Name
                    elif bs.Element:
                        print bs.Element.Id, bs.Element
                    else:
                        print bs.Element
    except:
        traceback.print_exc()
    pass

main()
```