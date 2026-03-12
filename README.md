import sqlite3
from datetime import datetime

# 1. Configuración de la base de datos
def init_db():
    conn = sqlite3.connect('gastos.db')
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS gastos (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            monto REAL NOT NULL,
            categoria TEXT NOT NULL,
            fecha TEXT NOT NULL
        )
    ''')
    conn.commit()
    conn.close()

# 2. Funciones principales
def agregar_gasto(monto, categoria):
    fecha = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    conn = sqlite3.connect('gastos.db')
    cursor = conn.cursor()
    cursor.execute('INSERT INTO gastos (monto, categoria, fecha) VALUES (?, ?, ?)', 
                   (monto, categoria, fecha))
    conn.commit()
    conn.close()
    print(f"✅ Gasto de ${monto} en '{categoria}' guardado.")

def ver_gastos():
    conn = sqlite3.connect('gastos.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM gastos')
    filas = cursor.fetchall()
    
    print("\n--- Historial de Gastos ---")
    total = 0
    for fila in filas:
        print(f"ID: {fila[0]} | ${fila[1]:.2f} | {fila[2]} | {fila[3]}")
        total += fila[1]
    print(f"---------------------------\nTOTAL ACUMULADO: ${total:.2f}")
    conn.close()

# 3. Interfaz de usuario simple
if __name__ == "__main__":
    init_db()
    while True:
        print("\n1. Agregar Gasto | 2. Ver Gastos | 3. Salir")
        opcion = input("Selecciona una opción: ")

        if opcion == '1':
            monto = float(input("Monto: "))
            cat = input("Categoría: ")
            agregar_gasto(monto, cat)
        elif opcion == '2':
            ver_gastos()
        elif opcion == '3':
            break
        else:
            print("Opción no válida.")
