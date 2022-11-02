import pandas as pd

#Abro el archivo
data = pd.read_excel("input/Datos Clientes.xlsx")

#Creo la clase cliente para guardar por cada cliente una lista de sus cuentas y sus saldos

class client:
    def __init__(self):
        self.nombre = ""
        self.cuentas=[]
        self.saldos=[]

#Creo una lista de clientes
clientes=[]

#Por cada fila de la tabla
for i, row in data.iterrows():
    #Veo si el cliente ya estaba definido en la lista, si es el caso le agrego la cuenta y saldo correspondiente a la fila actual
    clienteExistente = False
    for cliente in clientes:
        if(cliente.nombre==row[0]):
            clienteExistente = True
            cliente.cuentas.append(row[1])
            cliente.saldos.append(row[2])
            break
    if(clienteExistente == False):
        c = client()
        c.nombre = row[0]
        c.cuentas.append(row[1])
        c.saldos.append(row[2])
        clientes.append(c)

#Escribo los archivos de texto de salida
newline="\n\n"
str1="Buenas tardes; nos comunicamos con usted de para informarle los saldos que posee al día de la fecha en nuestro banco. A continuación la lista de cuentas que usted tiene en nuestro banco con su respectivo saldo:"
str2="Muchas gracias, no dude en comunicarse ante cualquier consulta."
str3="Saludos Cordiales."
for cliente in clientes:
    filename = "output/BANCO ABC " + cliente.nombre
    #Escribo el nuevo archivo de texto
    f = open(filename, 'w')
    f.write("\n" + "Sr/a :" + cliente.nombre + "\n" + str1 + newline + "Número de Cuenta              Saldo"  + "\n")
    #itero las cuentas y los saldos (los 2 arrays tienen la misma longitud) y escribo el documento
    total = 0
    for i in range(len(cliente.cuentas)):
        total = total + cliente.saldos[i]
        cuenta = str(cliente.cuentas[i])
        f.write(cuenta)
        saldo = str(cliente.saldos[i])
        f.write("              $" + saldo + ",00\n")
    strTotal = str(total)
    f.write("\n TOTAL:              $"+ strTotal + ",00\n")
    f.write("\n" + str2 + "\n" + str3)
    f.close()

#Podría hacerse un código más eficiente dando por sentado que la lista está en orden alfabético, que el máximo de cuentas por usuario es x u otras variables
#Pero este codigo es menos restrictivo respecto a los inputs
#Por otra parte, la plantilla fue "hardcodeada" en este código, dado que un código que obtenga la información directamente de la plantilla del correo podría ser muy sensible a cambios en la misma
