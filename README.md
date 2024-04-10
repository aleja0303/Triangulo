import math

class Punto2D:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return f'({self.x}, {self.y})'

class Vector2D:
    def __init__(self, punto1=None, punto2=None):
        if punto1 is None and punto2 is None:
            self.x = 0
            self.y = 0
        elif punto1 is not None and punto2 is None:
            self.x = punto1.x
            self.y = punto1.y
        elif punto1 is not None and punto2 is not None:
            self.x = punto2.x - punto1.x
            self.y = punto2.y - punto1.y
    
    def __str__(self):
        return f'Vector: ({self.x}, {self.y})'

class Triangulo2D:
    def __init__(self, *args):
        if len(args) == 3 and all(isinstance(p, Punto2D) for p in args):
            punto1, punto2, punto3 = args
            self.punto1 = punto1
            self.punto2 = punto2
            self.punto3 = punto3
            self.verificar_validez()
        elif len(args) == 1 and isinstance(args[0], Vector2D):
            vector_hipotenusa = args[0]
            self.punto1 = Punto2D(0, 0)
            self.punto2 = Punto2D(vector_hipotenusa.x, vector_hipotenusa.y)
            self.punto3 = Punto2D(vector_hipotenusa.x, 0)
            self.verificar_validez()
        else:
            raise ValueError("Argumentos inválidos para construir el triángulo")
    
    def verificar_validez(self):
        # Verificar que los puntos no estén alineados
        if (self.punto2.y - self.punto1.y) * (self.punto3.x - self.punto2.x) == (self.punto3.y - self.punto2.y) * (self.punto2.x - self.punto1.x):
            raise ValueError("Los puntos están alineados, no forman un triángulo válido")
    
    def calcular_lado(self, punto1, punto2):
        return math.sqrt((punto2.x - punto1.x)**2 + (punto2.y - punto1.y)**2)
    
    def calcular_angulo(self, lado1, lado2, lado3):
        return math.degrees(math.acos((lado2**2 + lado3**2 - lado1**2) / (2 * lado2 * lado3)))
    
    def calcular_perimetro(self):
        lado1 = self.calcular_lado(self.punto1, self.punto2)
        lado2 = self.calcular_lado(self.punto2, self.punto3)
        lado3 = self.calcular_lado(self.punto3, self.punto1)
        return lado1 + lado2 + lado3
    
    def calcular_area(self):
        lado1 = self.calcular_lado(self.punto1, self.punto2)
        lado2 = self.calcular_lado(self.punto2, self.punto3)
        lado3 = self.calcular_lado(self.punto3, self.punto1)
        s = (lado1 + lado2 + lado3) / 2
        return math.sqrt(s * (s - lado1) * (s - lado2) * (s - lado3))
    
    def mostrar_angulos(self):
        lado1 = self.calcular_lado(self.punto1, self.punto2)
        lado2 = self.calcular_lado(self.punto2, self.punto3)
        lado3 = self.calcular_lado(self.punto3, self.punto1)
        
        angulo1 = self.calcular_angulo(lado1, lado2, lado3)
        angulo2 = self.calcular_angulo(lado2, lado1, lado3)
        angulo3 = self.calcular_angulo(lado3, lado1, lado2)
        
        print(f"Ángulo entre lado 1 y lado 2: {angulo1:.2f} grados")
        print(f"Ángulo entre lado 2 y lado 3: {angulo2:.2f} grados")
        print(f"Ángulo entre lado 3 y lado 1: {angulo3:.2f} grados")

    def __str__(self):
        return f"Triangulo:\nPunto1: {self.punto1}\nPunto2: {self.punto2}\nPunto3: {self.punto3}"

# Ejemplo de uso
try:
    punto1 = Punto2D(0, 0)
    punto2 = Punto2D(0, 3)
    punto3 = Punto2D(4, 0)

    triangulo1 = Triangulo2D(punto1, punto2, punto3)
    print(triangulo1)
    print("Perímetro:", triangulo1.calcular_perimetro())
    print("Área:", triangulo1.calcular_area())
    triangulo1.mostrar_angulos()
    print()

    vector_hipotenusa = Vector2D(punto2, punto3)
    triangulo2 = Triangulo2D(vector_hipotenusa)
    print(triangulo2)
    print("Perímetro:", triangulo2.calcular_perimetro())
    print("Área:", triangulo2.calcular_area())
    triangulo2.mostrar_angulos()
except ValueError as e:
    print(e)
