from direct.showbase.ShowBase import ShowBase
from panda3d.core import Vec3
from direct.task import Task
from direct.actor.Actor import Actor
from panda3d.core import Point3

class MiJuegoDeAutos(ShowBase):
    def __init__(self):
        ShowBase.__init__(self)
        
        # Carga el modelo del entorno
        self.environ = self.loader.loadModel("models/environment")
        self.environ.reparentTo(self.render)
        self.environ.setScale(0.25, 0.25, 0.25)
        self.environ.setPos(-8, 42, 0)
        
        # Carga el modelo del auto
        self.auto = self.loader.loadModel("models/car")
        self.auto.reparentTo(self.render)
        self.auto.setScale(0.5, 0.5, 0.5)
        self.auto.setPos(0, 0, 0)
        
        # Configura la cámara
        self.camera.reparentTo(self.auto)
        self.camera.setPos(0, -10, 5)
        self.camera.lookAt(self.auto)
        
        # Configura los controles
        self.accept("arrow_left", self.gira_izquierda)
        self.accept("arrow_right", self.gira_derecha)
        self.accept("arrow_up", self.acelera)
        self.accept("arrow_down", self.frena)
        
        self.velocidad = Vec3(0, 0, 0)
        self.aceleracion = 1
        
        # Añade la tarea de actualización
        self.taskMgr.add(self.actualiza, "actualiza")
        
    def gira_izquierda(self):
        self.auto.setH(self.auto.getH() + 5)
        
    def gira_derecha(self):
        self.auto.setH(self.auto.getH() - 5)
        
    def acelera(self):
        self.velocidad += Vec3(0, self.aceleracion, 0)
        
    def frena(self):
        self.velocidad -= Vec3(0, self.aceleracion, 0)
        
    def actualiza(self, task):
        dt = globalClock.getDt()
        self.auto.setPos(self.auto.getPos() + self.velocidad * dt)
        return Task.cont

app = MiJuegoDeAutos()
app.run()
