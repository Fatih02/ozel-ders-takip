# ozel-ders-takip{
  "name": "ozel-ders-takip",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.0.0",
    "vite": "^4.3.9"
  }
}
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000
  }
})
<!DOCTYPE html>
<html lang="tr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="theme-color" content="#4f46e5" />
    <link rel="manifest" href="/manifest.json" />
    <title>Özel Ders Takip</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/index.js"></script>
  </body>
</html>
{
  "short_name": "DersTakip",
  "name": "Özel Ders Takip",
  "icons": [
    {
      "src": "/icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#4f46e5",
  "background_color": "#ffffff"
}
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("ders-takip-cache").then((cache) => {
      return cache.addAll(["/", "/index.html", "/manifest.json"]);
    })
  );
});

self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// Service worker'ı kayıt et
if ("serviceWorker" in navigator) {
  window.addEventListener("load", () => {
    navigator.serviceWorker
      .register("/service-worker.js")
      .then(() => console.log("Service Worker kayıt edildi."))
      .catch((err) => console.log("Service Worker kayıt hatası:", err));
  });
}
import React, { useState } from "react";

export default function App() {
  const [lessons, setLessons] = useState([]);
  const [student, setStudent] = useState("");
  const [date, setDate] = useState("");
  const [topic, setTopic] = useState("");

  const addLesson = () => {
    if (student && date && topic) {
      setLessons([...lessons, { student, date, topic }]);
      setStudent("");
      setDate("");
      setTopic("");
    }
  };

  return (
    <div style={{ padding: "20px", fontFamily: "sans-serif" }}>
      <h1>📚 Özel Ders Takip</h1>

      <div style={{ marginBottom: "10px" }}>
        <input
          type="text"
          placeholder="Öğrenci Adı"
          value={student}
          onChange={(e) => setStudent(e.target.value)}
        />
        <input
          type="date"
          value={date}
          onChange={(e) => setDate(e.target.value)}
        />
        <input
          type="text"
          placeholder="Konu"
          value={topic}
          onChange={(e) => setTopic(e.target.value)}
        />
        <button onClick={addLesson}>Ekle</button>
      </div>

      <table border="1" cellPadding="5" style={{ width: "100%" }}>
        <thead>
          <tr>
            <th>Öğrenci</th>
            <th>Tarih</th>
            <th>Konu</th>
          </tr>
        </thead>
        <tbody>
          {lessons.map((lesson, index) => (
            <tr key={index}>
              <td>{lesson.student}</td>
              <td>{lesson.date}</td>
              <td>{lesson.topic}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}
{
  "name": "ozel-ders-takip",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
