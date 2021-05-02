#### _Conexion pgAdmin4 -> Postgresql_

> Luego de abrir en el navegador pgAdmin seguir la receta:

 1. Servers
 2. Agregar nuevo Servidor
    1. General 
        1. Agregar un nombre que se te cante
    2.  Conexion
        1. paso Largo
            1. Nombre/Direc.Sevidor: va la IP de la BD. 
                1. Como Obtener la IP 
                    <pre>
                        ctrl+alt+t 
                        ps -ef | grep -i docker

                    output:
                        root         925       1  0 07:22 ?        00:00:14 /usr/bin/dockerd -H fd://root         940     925  0 07:22 ?        00:00:09 containerd --config /var/run/docker/containerd/containerd.toml --log-level info ramon       2164    2042  0 07:23 ?        00:00:00 /var/lib/snapd/snap/code/60/usr/share/code/code /home/ramon/.vscode/extensions/ms-azuretools.vscode-docker-1.11.0/dist/dockerfile-language-server-nodejs/lib/server.js --node-ipc --node-ipc --clientProcessId=2042 root       19543     925  0 10:25 ?        00:00:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 8091 -container-ip 192.168.80.2 -container-port 5432 root       19562       1  0 10:25 ?        00:00:00 /usr/bin/containerd-shim-runc-v2 -namespace moby -id 1fb2c473c7e666aa382fe934cbe8b31053f5d63273f0654d5ec24e76ae9166c1 -address /var/run/docker/containerd/containerd.sock root       19580     925  0 10:25 ?        00:00:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 8094 -container-ip 192.168.80.3 -container-port 443 root       19611     925  0 10:25 ?        00:00:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 8093 -container-ip 192.168.80.3 -container-port 80 root       19632     925  0 10:25 ?        00:00:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 8092 -container-ip 192.168.80.3 -container-port 5050 root       19656       1  0 10:25 ?        00:00:00 /usr/bin/containerd-shim-runc-v2 -namespace moby -id e7c32957e4c617dc854af59a3fa9d86d94b799105947128804c61d70e3c52217 -address /var/run/docker/containerd/containerd.sock

                    </pre>
                    > lo que importa: 

                ###### root       19543     925  0 10:25 ?        00:00:00 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 8091 -container-ip 192.168.80.2 -container-port 5432

                    <pre>
                        nmap 192.168.80.1/24
                    
                    output: 
                    Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-09 10:26 -03 Nmap scan report for 192.168.80.1 (192.168.80.1) Host is up (0.00029s latency). Not shown: 999 closed ports PORT     STATE SERVICE 8093/tcp open  unknown Nmap scan report for 192.168.80.2 (192.168.80.2) Host is up (0.00032s latency). Not shown: 999 closed ports PORT     STATE SERVICE 5432/tcp open  postgresql Nmap scan report for 192.168.80.3 (192.168.80.3) Host is up (0.00034s latency). Not shown: 998 closed ports PORT   STATE SERVICE 25/tcp open  smtp 80/tcp open  http Nmap done: 256 IP addresses (3 hosts up) scanned in 2.98 seconds 
                    </pre>

                    > la IP que necesitamos

                    ###### 192.168.80.2


            2. Puerto: el de la BD, ver si no esta mapeado
                <pre>
                    docker ps
                
                output:
                1fb2c473c7e6   postgres         "docker-entrypoint.s…"   22 minutes ago   Up 22 minutes   0.0.0.0:8091->5432/tcp                                                docker_db
                </pre>

                > Puerto de salida 8091

            3. Completar los datos siguentes

            4. Guardar.

            5. Nota
            > Desventajas: cada vez que reiniciamos los servidores se le asignara una nueva IP, a lo que lleva a crear una nueva conexion con pgAdmin. Un embole.
        2. Paso Corto
            1. Nombre/Direccion Servidor: colocar el nombre asigando en los _links_ declarados en Docker-Composer file.

                <pre>
                    #Docker-compose.yml
                    
                    links:
                        - "nombreBD:nombreDelLink"
                </pre>