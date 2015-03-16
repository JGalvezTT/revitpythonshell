# Introduction #

This is the source of a module I use to make certain operations with the RevitAPI easier to access from IronPython. This will probably grow as time goes on and eventually will be part of an Installer mechanism for sharing useful RevitPythonShell scripts.


# Details #
Copy this code into a file called `geomutil.py` in your search path (e.g. `C:\RevitPython\geomutil.py`:

```
'''
geomutil.py

Some utility methods for working with 3d vectors
'''
import math
class Point(object):
    def __init__(self, x=0, y=0, z=0):
        self.x = float(x)
        self.y = float(y)
        self.z = float(z)
    def __str__(self):
        return "Point(%0.2f, %0.2f, %0.2f)" % (self.x, self.y, self.z)
    def __repr__(self):
        return "Point(%s, %s, %s)" % (self.x, self.y, self.z)


class Vector(object):
    def __init__(self, p1=Point(), p2=Point()):
        self.x = float(p2.x - p1.x)
        self.y = float(p2.y - p1.y)
        self.z = float(p2.z - p1.z)

    def vectorProduct(self, v):
        result = Vector()
        result.x = self.z * v.y - self.y * v.z
        result.y = self.x * v.z - self.z * v.x
        result.z = self.y * v.x - self.x * v.y
        return result

    def scalarProduct(self, v):
        return self.x * v.x + self.y * v.y + self.z * v.z

    def __str__(self):
        return str((self.x, self.y, self.z))

    def length(self):
        return math.sqrt(self.x**2 + self.y**2 + self.z**2)

class Line(object):
    def __init__(self, p1, p2):
        self.p1 = p1
        self.p2 = p2
    def distance(self, p):
        '''distance between this line and a point p'''
        return Vector(self.p1, p).vectorProduct(Vector(self.p1, self.p2)).length() / Vector(self.p1, self.p2).length()
    def length(self):
        return Vector(self.p1, self.p2).length()
    def isColinear(self, other):
        return self.distance(other.p1) < 0.001
    def __str__(self):
        return "Line(%s, %s)" % (self.p1, self.p2)
    def __repr__(self):
        return "Line(%s, %s)" % (self.p1, self.p2)

def linesAreColinear(lines):
    '''each line in sequence lines must be colinear with the other lines'''
    test = lines[0]
    for l in lines:
        if not l.isColinear(test):
            return False
    return True

```