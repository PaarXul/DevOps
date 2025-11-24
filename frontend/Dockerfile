# Etapa de build de Angular
FROM node:18-alpine AS build
WORKDIR /app

# Copia package files y descarga dependencias
COPY package*.json ./
RUN npm ci

# Copia el código (ajusta la ruta en tu repo real)
COPY . .

# Variable opcional para el nombre del proyecto Angular
# Si tu salida es dist/<app-name>, puedes definir APP_NAME via --build-arg
ARG APP_NAME=app

# Compila para producción
RUN npm run build -- --configuration=production || npm run build

# Etapa de runtime con NGINX
FROM nginx:stable-alpine

# Copia build al NGINX html (ajusta si tu dist es diferente)
# Si tu Angular genera dist/<app-name>, intenta copiarlo dinámicamente
# Si no conoces el nombre exacto, especifica en tiempo de build con --build-arg APP_NAME=<nombre>
COPY --from=build /app/dist /usr/share/nginx/html

# Copia configuración NGINX por defecto (opcional)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
