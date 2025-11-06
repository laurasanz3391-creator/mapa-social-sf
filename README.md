# mapa-social-sf
# Crear carpeta del proyecto y entrar
mkdir mapa-social-sf
cd mapa-social-sf

# Inicializar Git
git init

# Crear estructura de carpetas
mkdir backend frontend database
mkdir frontend/src frontend/src/components

# --- Backend ---
echo "import express from 'express';
import cors from 'cors';
import dotenv from 'dotenv';
import { createClient } from '@supabase/supabase-js';
dotenv.config();
const app = express();
app.use(cors());
app.use(express.json());
const supabase = createClient(process.env.SUPABASE_URL, process.env.SUPABASE_KEY);
app.get('/', (req,res)=>res.send('API backend funcionando'));
app.listen(process.env.PORT||5000, ()=>console.log('Servidor corriendo'));" > backend/server.js

echo '{
  "name": "backend",
  "version": "1.0.0",
  "type": "module",
  "scripts": { "start": "node server.js" },
  "dependencies": { "express": "^4.18.2", "cors": "^2.8.5", "dotenv": "^16.3.1", "@supabase/supabase-js": "^2.28.0" }
}' > backend/package.json

echo "SUPABASE_URL=tu_supabase_url
SUPABASE_KEY=tu_supabase_key
JWT_SECRET=una_clave_segura
PORT=5000" > backend/.env.example

# --- Frontend ---
echo "import React from 'react';
import Mapa from './components/Mapa';
export default function App() { return <div><h1>Mapa Social SF</h1><Mapa /></div>; }" > frontend/src/App.jsx

echo "import { MapContainer, TileLayer } from 'react-leaflet';
import 'leaflet/dist/leaflet.css';
export default function Mapa() {
  const centroSantaFe = [-31.0, -60.7];
  return (<MapContainer center={centroSantaFe} zoom={7} style={{height:'80vh',width:'100%'}}>
    <TileLayer url='https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png'/>
  </MapContainer>);
}" > frontend/src/components/Mapa.jsx

echo '{
  "name": "frontend",
  "version": "1.0.0",
  "private": true,
  "scripts": { "dev": "vite", "build": "vite build", "preview": "vite preview" },
  "dependencies": { "react": "^18.2.0", "react-dom": "^18.2.0", "react-leaflet": "^4.2.1", "leaflet": "^1.9.4" },
  "devDependencies": { "vite": "^4.4.9" }
}' > frontend/package.json

# --- Database ---
echo "CREATE TABLE iglesias (
  id SERIAL PRIMARY KEY,
  nombre VARCHAR(150),
  direccion TEXT,
  latitud DECIMAL(9,6),
  longitud DECIMAL(9,6)
);
CREATE TABLE personas (
  id SERIAL PRIMARY KEY,
  nombre VARCHAR(100),
  apellido VARCHAR(100),
  dni VARCHAR(20),
  direccion TEXT,
  latitud DECIMAL(9,6),
  longitud DECIMAL(9,6),
  id_iglesia INT REFERENCES iglesias(id)
);
CREATE TABLE ayudas (
  id SERIAL PRIMARY KEY,
  id_persona INT REFERENCES personas(id),
  fecha DATE,
  tipo_ayuda TEXT,
  observaciones TEXT
);
CREATE TABLE pedidos (
  id SERIAL PRIMARY KEY,
  id_persona INT REFERENCES personas(id),
  fecha DATE,
  descripcion TEXT
);
CREATE TABLE usuarios (
  id SERIAL PRIMARY KEY,
  nombre VARCHAR(100),
  email VARCHAR(150) UNIQUE,
  password_hash TEXT,
  rol VARCHAR(20) CHECK (rol IN ('admin','voluntario','publico'))
);" > database/init.sql

# --- README ---
echo "# Mapa Social SF

## Backend
- Node.js + Express + Supabase
- Rutas: /personas, /iglesias, /ayudas, /pedidos, /auth
- Variables de entorno: .env.example

## Frontend
- React + Leaflet
- Mapa interactivo centrado en Santa Fe, Argentina
- Formulario de personas, ayudas y pedidos
- Login con JWT

## Despliegue
- Backend: Render
- Frontend: Vercel

## Usuario admin inicial
- Email: admin@sistema.com
- Contraseña: Admin123!" > README.md

# --- Subir a GitHub ---
git add .
git commit -m "Primer commit: proyecto mapa-social-sf"
git remote add origin https://github.com/laurasanz3391-creador/mapa-social-sf.git
git branch -M main
git push -u origin main
