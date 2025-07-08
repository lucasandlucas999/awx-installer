# AWX Operator Installer

Un script automatizado para instalar y configurar múltiples instancias de AWX Operator en Ubuntu utilizando Kind (Kubernetes in Docker).

## 📋 Descripción

Este script bash automatiza la instalación de AWX Operator en clusters de Kubernetes locales usando Kind. Permite crear múltiples instancias de AWX de forma independiente, cada una en su propio cluster y puerto específico.

## 🚀 Características

- **Instalación automatizada** de dependencias (Docker, kubectl, kind, kustomize)
- **Múltiples instancias** de AWX en diferentes clusters
- **Configuración automática** de puertos y namespaces
- **Interfaz interactiva** fácil de usar
- **Configuración de firewall** automática (UFW)
- **Validación** de prerequisitos y estado de servicios

## 📦 Prerequisitos

- Ubuntu 18.04 o superior
- Acceso a internet
- Permisos sudo
- Al menos 4GB de RAM libre
- Docker (se instala automáticamente si no está presente)

## 🛠️ Instalación

### Descarga del script

```bash
# Clonar repositorio (si está en GitHub)
git clone https://github.com/lucasandlucas999/awx-installer.git
cd awx

# O descargar directamente
wget https://github.com/lucasandlucas999/awx-installer.git/awx-installer.sh
chmod +x awx-installer.sh
```

### Ejecución

```bash
./awx-installer.sh
```

## 📖 Uso

### 1. Primera instalación

Ejecuta el script y selecciona la opción `1`:

```bash
./awx-installer.sh
```

- **Cluster name**: Nombre del cluster Kind (por defecto: `awx-cluster`)
- **Puerto**: Puerto para acceder a AWX (por defecto: `30080`)

### 2. Instancias adicionales

Para añadir más instancias de AWX, ejecuta el script y selecciona la opción `2`:

```bash
./awx-installer.sh
```

Proporciona:
- **Nombre del cluster**: Nombre único para el nuevo cluster
- **Nombre de la instancia**: Nombre para la instancia AWX
- **Namespace**: Namespace de Kubernetes (por defecto: `awx-{nombre-instancia}`)
- **Puerto**: Puerto único para la nueva instancia

### 3. Ejemplo de configuración múltiple

```bash
# Primera instancia
Cluster: awx-cluster
Puerto: 30080
URL: http://tu-ip:30080

# Segunda instancia
Cluster: awx-cluster2
Puerto: 30081
URL: http://tu-ip:30081

# Tercera instancia
Cluster: awx-cluster3
Puerto: 30082
URL: http://tu-ip:30082
```

## 🔧 Configuración técnica

### Componentes instalados

- **Docker**: Motor de contenedores
- **kubectl**: Cliente de Kubernetes
- **kind**: Kubernetes in Docker
- **kustomize**: Herramienta de personalización de Kubernetes
- **AWX Operator**: Versión 2.19.1

### Estructura de archivos generados

```
./
├── awx-{nombre}/
│   ├── paso1/
│   │   └── kustomization.yaml
│   └── paso2/
│       ├── kustomization.yaml
│       └── awx.yaml
└── kind-config-{cluster}.yaml
```

## 🔐 Acceso a AWX

Después de la instalación exitosa, el script mostrará:

```
==========================================
AWX instalado correctamente
Nombre: awx
Namespace: awx
URL de acceso: http://192.168.1.100:30080
Usuario: admin
Contraseña: [contraseña-generada]
==========================================
```

## 🛡️ Seguridad

- Las contraseñas se generan automáticamente
- Los puertos se abren automáticamente en UFW
- Cada instancia está aislada en su propio cluster
- Los secretos se almacenan en Kubernetes secrets

## 📊 Monitoreo

### Verificar estado de la instalación

```bash
# Ver logs del operador
kubectl logs -f deployments/awx-operator-controller-manager -c awx-manager -n awx

# Ver pods
kubectl get pods -n awx

# Ver servicios
kubectl get svc -n awx
```

### Comandos útiles

```bash
# Listar clusters Kind
kind get clusters

# Obtener contraseña de admin
kubectl get secret awx-admin-password -o jsonpath="{.data.password}" -n awx | base64 --decode

# Ver estado de AWX
kubectl get awx -n awx
```

## 🔄 Gestión de clusters

### Eliminar una instancia

```bash
# Eliminar cluster completo
kind delete cluster --name awx-cluster

# Solo eliminar AWX (mantener cluster)
kubectl delete awx awx -n awx
```

### Cambiar contexto entre clusters

```bash
# Listar contextos
kubectl config get-contexts

# Cambiar contexto
kubectl config use-context kind-awx-cluster2
```

## 🐛 Solución de problemas

### Problemas comunes

1. **Error de permisos Docker**:
   ```bash
   sudo usermod -aG docker $USER
   # Reiniciar sesión
   ```

2. **Puerto ya en uso**:
   ```bash
   # Verificar puertos en uso
   sudo netstat -tulpn | grep :30080
   ```

3. **Pods no se inician**:
   ```bash
   # Verificar recursos
   kubectl top nodes
   kubectl describe pod -n awx
   ```

### Logs de depuración

```bash
# Logs del operador
kubectl logs -f deployments/awx-operator-controller-manager -c awx-manager -n awx

# Logs de AWX
kubectl logs -f deployment/awx-web -n awx
```

## 🤝 Contribución

1. Fork el repositorio
2. Crea una branch para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. Commit tus cambios (`git commit -am 'Añadir nueva funcionalidad'`)
4. Push a la branch (`git push origin feature/nueva-funcionalidad`)
5. Crea un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.

## 👤 Autor

**Lucas Acuña**

## 🙏 Agradecimientos

- [Ansible AWX](https://github.com/ansible/awx) por el proyecto AWX
- [AWX Operator](https://github.com/ansible/awx-operator) por el operador de Kubernetes
- [Kind](https://kind.sigs.k8s.io/) por la herramienta de clusters locales

## 📚 Referencias

- [Documentación oficial de AWX](https://ansible.readthedocs.io/projects/awx/en/latest/)
- [AWX Operator GitHub](https://github.com/ansible/awx-operator)
- [Kind Documentation](https://kind.sigs.k8s.io/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)

---

⭐ Si este proyecto te resulta útil, ¡dale una estrella en GitHub!