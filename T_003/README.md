# Projet

Faire le crud intialement dans le projet T_002 dans cette version backend.

1. Création d'une tâche
2. Mise à jour d'une tâche
3. Suppression d'un tâche
4. Récupération de toutes les tâches
5. Récupération d'un seule tâche


Un appel API HTTP permet à ton app React de communiquer avec un serveur (backend ou API tierce) pour :

récupérer des données (GET)
envoyer des données (POST)
mettre à jour (PUT/PATCH)
supprimer (DELETE)


Exemple d'API REST :
GET https://api.exemple.com/utilisateurs

Utiliser fetch en React

Exemple avec useEffect et useState :

import { useEffect, useState } from "react";

interface Utilisateur {
  id: number;
  nom: string;
}

function ListeUtilisateurs() {
  const [users, setUsers] = useState<Utilisateur[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [erreur, setErreur] = useState<string | null>(null);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((res) => {
        if (!res.ok) throw new Error("Erreur serveur");
        return res.json();
      })
      .then((data) => setUsers(data))
      .catch((err) => setErreur(err.message))
      .finally(() => setLoading(false));
  }, []);

  if (loading) return <p>Chargement...</p>;
  if (erreur) return <p>Erreur : {erreur}</p>;

  return (
    <ul>
      {users.map((u) => (
        <li key={u.id}>{u.nom}</li>
      ))}
    </ul>
  );
}



Utiliser axios (alternative à fetch)
npm install axios

import axios from "axios";

useEffect(() => {
  axios
    .get<Utilisateur[]>("https://jsonplaceholder.typicode.com/users")
    .then((res) => setUsers(res.data))
    .catch((err) => setErreur(err.message))
    .finally(() => setLoading(false));
}, []);



Avantages de axios :

Moins de code répétitif (pas besoin de .res.json())
Meilleure gestion des erreurs
Support de requêtes avancées (intercepteurs, annulation…)



Schéma : cycle d’un appel API dans React

[ useEffect ] 
     ↓
[ fetch / axios ] 
     ↓
[ useState ]
     ↓
[ Affichage conditionnel ]




Astuce : créer un fichier API centralisé

// src/api.ts
import axios from "axios";

const api = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com",
});

export default api;


Puis dans le composant :

import api from "./api";

api.get("/users").then((res) => setUsers(res.data));
