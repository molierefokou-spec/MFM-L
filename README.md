import streamlit as st
import random
from mfm_l_omni import MFM_L_Omni_Protocol # On importe ta classe

# Configuration de la page
st.set_page_config(page_title="MFM-L OMNI", page_icon="🍷")

# Initialisation du protocole dans la session
if "mfml" not in st.session_state:
    st.session_state.mfml = MFM_L_Omni_Protocol()
    st.session_state.messages = []

st.title("🍷 MFM-L OMNI : Protocole Dual")
st.caption("Mode : Mentor Stoïcien 💼 | Magnétique 🍷")

# Affichage de la barre d'énergie en haut
energy_bar = st.progress(st.session_state.mfml.energy_joules / 100)
st.write(f"⚡ Énergie : {st.session_state.mfml.energy_joules:.1f}J")

# Affichage de l'historique des messages
for message in st.session_state.messages:
    with st.chat_message(message["role"]):
        st.markdown(message["content"])

# Zone d'entrée utilisateur
if prompt := st.chat_input("Alors, Mystère, on s'attaque à quoi ?"):
    # Ajouter le message utilisateur
    st.session_state.messages.append({"role": "user", "content": prompt})
    with st.chat_message("user"):
        st.markdown(prompt)

    # Générer la réponse de MFM-L
    response = st.session_state.mfml.executer_cycle(prompt)
    
    # Ajouter la réponse de MFM-L
    with st.chat_message("assistant"):
        st.markdown(response)
    st.session_state.messages.append({"role": "assistant", "content": response})
    
    # Forcer le rafraîchissement pour l'énergie
    st.rerun()
