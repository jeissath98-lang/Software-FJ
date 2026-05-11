from abc import ABC, abstractmethod
import logging

# CONFIGURACIÓN DE LOGS

logging.basicConfig(
    filename="logs.txt",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

print("\n===== SISTEMA SOFTWARE FJ =====\n")

# CLASE ABSTRACTA

class Entidad(ABC):

    def __init__(self, id):
        self._id = id

    @abstractmethod
    def mostrar_info(self):
        pass

# CLASE CLIENTE

class Cliente(Entidad):

    def __init__(self, id, nombre, email, telefono):
        super().__init__(id)

        if len(nombre) < 3:
            raise ValueError("Nombre demasiado corto")

        if "@" not in email:
            raise ValueError("Email inválido")

        if not telefono.isdigit():
            raise ValueError("Teléfono inválido")

        self.__nombre = nombre
        self.__email = email
        self.__telefono = telefono

    def mostrar_info(self):
        return f"Cliente: {self.__nombre}"

# CLASE ABSTRACTA SERVICIO

class Servicio(ABC):

    def __init__(self, nombre, precio):
        if precio <= 0:
            raise ValueError("Precio inválido")

        self.nombre = nombre
        self.precio = precio

    @abstractmethod
    def calcular_costo(self):
        pass

    @abstractmethod
    def descripcion(self):
        pass

# SERVICIO BÁSICO

class ServicioBasico(Servicio):

    def calcular_costo(self):
        return self.precio * 1.10

    def descripcion(self):
        return "Servicio Básico"

# SERVICIO PREMIUM

class ServicioPremium(Servicio):

    def calcular_costo(self):
        return self.precio * 1.25

    def descripcion(self):
        return "Servicio Premium"

# SERVICIO EMPRESARIAL

class ServicioEmpresarial(Servicio):

    def calcular_costo(self):
        return self.precio * 1.50 - 20

    def descripcion(self):
        return "Servicio Empresarial"

# CLASE RESERVA

class Reserva:

    def __init__(self, cliente, servicio, duracion):

        if cliente is None:
            raise ValueError("Cliente no válido")

        if duracion <= 0:
            raise ValueError("Duración inválida")

        self.cliente = cliente
        self.servicio = servicio
        self.duracion = duracion
        self.estado = "Pendiente"

    def confirmar(self):

        try:
            self.estado = "Confirmada"
            print("Reserva confirmada")
            logging.info("Reserva confirmada")

        except Exception as e:
            print("Error al confirmar")
            logging.error(e)

    def cancelar(self):

        try:
            self.estado = "Cancelada"
            print("Reserva cancelada")
            logging.warning("Reserva cancelada")

        except Exception as e:
            print("Error al cancelar")
            logging.error(e)

    def procesar(self):

        try:
            costo = self.servicio.calcular_costo()

        except Exception as e:
            print("Error procesando reserva")
            logging.error(e)

        else:
            print("Costo calculado: {costo}")

        finally:
            print("Proceso finalizado")

    # Sobrecarga simulada
    def calcular_total(self, impuesto=0.19, descuento=0):

        subtotal = self.servicio.calcular_costo() * self.duracion

        total = subtotal + (subtotal * impuesto) - descuento

        return total

# 10 OPERACIONES

# OPERACIÓN 1
print("\nOPERACIÓN 1: Cliente válido")

try:
    cliente1 = Cliente(1, "Fernanda", "fernanda@gmail.com", "123456789")
    print(cliente1.mostrar_info())

except Exception as e:
    print(e)


# OPERACIÓN 2
print("\nOPERACIÓN 2: Cliente inválido")

try:
    cliente2 = Cliente(2, "Fe", "correo", "abc")

except Exception as e:
    print("Error", e)
    logging.error(e)


# OPERACIÓN 3
print("\nOPERACIÓN 3: Servicio Básico")

try:
    servicio1 = ServicioBasico("Diseño Web", 100)
    print(servicio1.descripcion())

except Exception as e:
    print(e)


# OPERACIÓN 4
print("\nOPERACIÓN 4: Servicio Premium")

try:
    servicio2 = ServicioPremium("App Móvil", 300)
    print(servicio2.descripcion())

except Exception as e:
    print(e)


# OPERACIÓN 5
print("\nOPERACIÓN 5: Servicio Empresarial")

try:
    servicio3 = ServicioEmpresarial("Sistema ERP", 500)
    print(servicio3.descripcion())

except Exception as e:
    print(e)


# OPERACIÓN 6
print("\nOPERACIÓN 6: Servicio inválido")

try:
    servicio_error = ServicioBasico("Error", -50)

except Exception as e:
    print("Error", e)
    logging.error(e)


# OPERACIÓN 7
print("\nOPERACIÓN 7: Reserva válida")

try:
    reserva1 = Reserva(cliente1, servicio1, 2)
    reserva1.confirmar()

except Exception as e:
    print(e)


# OPERACIÓN 8
print("\nOPERACIÓN 8: Procesar reserva")

try:
    reserva1.procesar()

except Exception as e:
    print(e)


# OPERACIÓN 9
print("\nOPERACIÓN 9: Calcular total")

try:
    total = reserva1.calcular_total(descuento=10)
    print("Total", total)

except Exception as e:
    print(e)


# OPERACIÓN 10
print("\nOPERACIÓN 10: Reserva inválida")

try:
    reserva_error = Reserva(None, servicio2, -1)

except Exception as e:
    print("Error", e)
    logging.error(e)


print("\n===== FIN DEL SISTEMA =====")
