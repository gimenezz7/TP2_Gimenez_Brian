# Base de Conocimiento



## Problemas Crear una VM - (Permisos Para Renombrar)

- [ ] **Problemas Crear una VM - (Permisos Para Renombrar)**
    > Tras ejecutar "vagrant up" sale un error diciendo que ya existe una VM con dicho nombre.  </br>
    > Tambien que NO puede Renombrar un directorio "c:\user\ **EL_USUARIO_DE_USTEDES**"  </br>
    > Por si fuera poco, llego a generar una VM en virtualbox a la cual no le pudo cambiar el nombre.  
   <details>
     <summary>&emsp; <Mostrar/Ocultar> - [Click para ver] -> Screen del Error</summary>
   <div>
   <table>
      <tr>
         <td><img src=".img/vagrant_up_Error_01.png" width="100%" align="center"></td>
      </tr>
      <tr>
         <td><img src=".img/vagrant_up_Error_02_01.png" width="50%" align="center"></td>
      </tr>
   </table>
   </div>
   </details>

   **Solucion:**
   - Paso1 - Eliminamos todo Rastro de la maquina virtual y metadata de vagrant
```sh
#- Verificamos si en VirtualBox existe una VM con dicho Nombre.
#- Eliminamos la VM con el comando o Manualmente desde VirtualBox:
vagrant destroy -f 
#- Eliminar carpeta de metadata de vagrant
rm -rf .vagrant
```
   - Paso2 - Modificamos en la configuracion de VirtualBox el path por defecto donde se alojan las VM
<div style="margin-left: 40px;">
   <details>
     <summary>&emsp; <Mostrar/Ocultar> - [Click para ver] -> Screen cambiar preferencias</summary>
   <div>
   <table>
      <tr>
         <td><img src=".img/Preferencias_01.png" width="50%" align="center"></td>
      </tr>
      <tr>
         <td><img src=".img/Preferencias_02.png" width="90%" align="center"></td>
      </tr>
   </table>
   </div>
   </details>
</div>

> NOTA: La mayoria de los Errores al levantar VM se soluciona de esta forma.  


## Problemas Crear una VM - (Problema con la red)

- [ ] **Problemas Crear una VM - (Problema con la red)**
    > Tras ejecutar "vagrant up" aparece un error  de red ( `hostonly_config': undefined method `read_host_only_networks') [Ver Issue](https://github.com/hashicorp/vagrant/issues/13655)  
   <details>
     <summary>&emsp; <Mostrar/Ocultar> - [Click para ver] -> Screen del Error</summary>
   <div>
         <td><img src=".img/Vagrant_up_Error_03_Network.png" width="100%" align="center"></td>
   </div>
   </details>

   **Solucion:**
   - Paso1 - Buscamos en el archivo `Vagrantfile` la configuracion de red.. 
```sh
# Linea anterior
config.vm.network "private_network", :name => '', ip: "192.168.56.2"
```
> El numero de IP puede variar entre archivos...(resperar la que estaba)

   - Paso2 - Agregando en dicha linea la opcion `, virtualbox__intnet: true`
```sh
# Nueva Linea
 config.vm.network "private_network", :name => '', ip: "192.168.56.2", virtualbox__intnet: true
```
> Esto le indica explícitamente a Vagrant que configure la red privada como una red interna de VirtualBox,  
> evitando el intento fallido de interactuar con las redes "solo-anfitrión" que causaba el error  


## Problemas Crear una VM - (No existe el adaptador de red)

- [ ] **Problemas Crear una VM - (No existe el adaptador de red)**
    > Tras ejecutar "vagrant up" aparece un error  de red  
    > (Stderr: VBoxManage.exe: error: Failed to open/create the internal network 'HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter' (VERR_INTNET_FLT_IF_NOT_FOUND).)  
   <details>
     <summary>&emsp; <Mostrar/Ocultar> - [Click para ver] -> Screen del Error</summary>
   <div>
         <td><img src=".img/Vagrant_up_Error_04_Network_Not-Found.png" width="80%" align="center"></td>
   </div>
   </details>

   **Solucion:**
     - Abri VirtualBox (Todas las VM apagadas)
     - Ir a "Archivo" -> "Herramientas" -> "Administrador de red"
     - Click en "Eliminar" para borrar todos los adaptadores de red que figuren.
     - Clik "Crear", para Crear un nuevo adaptador.
     - Configurar el Nuevo adaptador de red como: 192.168.56.1/24  (IP: 192.168.56.1 Mascara de red: 255.255.255.0)
   <details>
     <summary>&emsp; <Mostrar/Ocultar> - [Click para ver] -> Screen de la configuracion</summary>
   <div>
         <td><img src=".img/VirtualBox_Config_NET.jpg" width="60%" align="center"></td>
   </div>
   </details>

