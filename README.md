from abc import ABC, abstractmethod
import logging
from datetime import datetime

logging.basicConfig(
    filename="logs.txt",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

class Entidad(ABC):
    def __init__(self, id):
        self._id = id

    @abstractmethod
    def mostrar_info(self):
        pass


class Cliente(Entidad):
    def __init__(self, id, nombre, email, telefono):
        super().__init__(id)
        self.__nombre = self.validar_nombre(nombre)
        self.__email = self.validar_email(email)
        self.__telefono = self.validar_telefono(telefono)

    def validar_nombre(self, nombre):
        if not nombre or len(nombre) < 3:
            raise ValueError("Nombre inválido")
        return nombre

    def validar_email(self, email):
        if "@" not in email:
            raise ValueError("Email inválido")
        return email

    def validar_telefono(self, telefono):
        if not telefono.isdigit():
            raise ValueError("Teléfono inválido")
        return telefono

    def mostrar_info(self):
        return f"Cliente: {self.__nombre}, Email: {self.__email}"

    # Encapsulación
    def get_nombre(self):
        return self.__nombre


class Servicio(ABC):
    def __init__(self, nombre, precio_base):
        self.nombre = nombre
        self.precio_base = precio_base

    @abstractmethod
    def calcular_costo(self):
        pass

    @abstractmethod
    def descripcion(self):
        pass


class ServicioBasico(Servicio):
    def calcular_costo(self):
        return self.precio_base * 1.10  # impuesto

    def descripcion(self):
        return "Servicio básico"


class ServicioPremium(Servicio):
    def calcular_costo(self):
        return self.precio_base * 1.25

    def descripcion(self):
        return "Servicio premium con beneficios"


class ServicioEmpresarial(Servicio):
    def calcular_costo(self):
        return self.precio_base * 1.50 - 20  # descuento

    def descripcion(self):
        return "Servicio empresarial con descuento"


class Reserva:
    def __init__(self, cliente, servicio, duracion):
        self.cliente = cliente
        self.servicio = servicio
        self.duracion = duracion
        self.estado = "pendiente"

    def confirmar(self):
        try:
            if self.estado != "pendiente":
                raise Exception("No se puede confirmar")
            self.estado = "confirmada"
            logging.info("Reserva confirmada")
        except Exception as e:
            logging.error(f"Error al confirmar: {e}")

    def cancelar(self):
        try:
            self.estado = "cancelada"
            logging.info("Reserva cancelada")
        except Exception as e:
            logging.error(f"Error al cancelar: {e}")

    def procesar(self):
        try:
            costo = self.servicio.calcular_costo()
        except Exception as e:
            logging.error(f"Error en cálculo: {e}")
        else:
            print(f"Costo calculado: {costo}")
        finally:
            print("Proceso finalizado")

    # Sobrecarga simulada
    def calcular_total(self, impuesto=0.19, descuento=0):
        costo = self.servicio.calcular_costo() * self.duracion
        total = costo + (costo * impuesto) - descuento
        return total


def main():
    try:
        # 1. Cliente válido
        c1 = Cliente(1, "Ana", "ana@email.com", "123456")

        # 2. Cliente inválido
        try:
            c2 = Cliente(2, "A", "correo", "abc")
        except Exception as e:
            logging.error(e)

        # 3. Servicio válido
        s1 = ServicioBasico("Basico", 100)

        # 4. Servicio premium
        s2 = ServicioPremium("Premium", 200)

        # 5. Servicio empresarial
        s3 = ServicioEmpresarial("Empresarial", 300)

        # 6. Reserva válida
        r1 = Reserva(c1, s1, 2)
        r1.confirmar()
        r1.procesar()

        # 7. Reserva cancelada
        r2 = Reserva(c1, s2, 1)
        r2.cancelar()

        # 8. Error en servicio
        try:
            s_error = ServicioBasico("Error", -100)
        except Exception as e:
            logging.error(e)

        # 9. Reserva con cálculo total
        print(r1.calcular_total())

        # 10. Reserva inválida
        try:
            r3 = Reserva(None, s3, -1)
            r3.procesar()
        except Exception as e:
            logging.error(e)

    except Exception as e:
        logging.critical(f"Error crítico: {e}")


if __name__ == "__main__":
    main()
