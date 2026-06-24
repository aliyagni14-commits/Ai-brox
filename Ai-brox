import streamlit as st
import urllib.parse
import re
from datetime import datetime

st.set_page_config(page_title="Asisten AI Aliy", page_icon="🤖", layout="centered")
st.title("🤖 Asisten AI Aliy (Versi Web)")
st.write("Ketik perintah atau obrolan santai di bawah ini, lalu tekan Enter!")

if "riwayat_chat" not in st.session_state:
    st.session_state.riwayat_chat = []

def proses_ai_web(perintah_asli):
    perintah = perintah_asli.lower().strip()
    respon_ai, redirect_url = "", None

    if perintah == "buka whatsapp":
        respon_ai, redirect_url = "Membuka WhatsApp...", "https://whatsapp.com"
    elif perintah == "buka google":
        respon_ai, redirect_url = "Membuka beranda Google...", "https://google.com"
    elif perintah == "buka roblox":
        respon_ai, redirect_url = "Membuka aplikasi Roblox...", "roblox://"
    elif perintah == "buka duolingo":
        respon_ai, redirect_url = "Membuka aplikasi Duolingo...", "duolingo://"
    elif perintah in ["buka youtube", "buka yt"]:
        respon_ai, redirect_url = "Membuka YouTube...", "vnd.youtube://"
    elif perintah in ["buka mobile legends", "buka ml"]:
        respon_ai, redirect_url = "Membuka game Mobile Legends!", "mobilelegends://"
    elif perintah == "buka chatgpt":
        respon_ai, redirect_url = "Membuka ChatGPT resmi...", "https://chatgpt.com"
    elif perintah.startswith("kirim wa ke "):
        try:
            sisa_teks = perintah_asli[12:].strip()
            match_pesan = re.search(r'\s+pesan\s+', sisa_teks, flags=re.IGNORECASE)
            if match_pesan:
                start_idx, end_idx = match_pesan.span()
                nomor_hp = sisa_teks[:start_idx].strip()
                isi_pesan = sisa_teks[end_idx:].strip()
                if nomor_hp.startswith("0"): nomor_hp = "62" + nomor_hp[1:]
                respon_ai = f"Menyiapkan pesan otomatis ke nomor {nomor_hp}..."
                pesan_encoded = urllib.parse.quote(isi_pesan)
                redirect_url = f"https://whatsapp.com{nomor_hp}&text={pesan_encoded}"
            else: respon_ai = "Format salah. Gunakan: kirim wa ke [nomor] pesan [isi pesan]"
        except: respon_ai = "Gagal memproses nomor WhatsApp."
    elif "jam berapa" in perintah or "menit berapa" in perintah:
        respon_ai = f"Saat ini waktu menunjukkan pukul {datetime.now().strftime('%H:%M')} WIB."
    elif perintah.startswith("cari ") or perintah.startswith("apa ") or perintah.startswith("pengen cari "):
        keyword = perintah
        for k in ["pengen cari ", "cari ", "apa "]:
            if keyword.startswith(k): keyword = keyword.replace(k, "", 1).strip(); break
        if keyword:
            q_final = "apa " + keyword if perintah.startswith("apa ") else keyword
            respon_ai, redirect_url = f"Mencari tentang '{q_final}' di Google...", f"https://google.com/search?q={urllib.parse.quote(q_final)}"
        else: respon_ai = "Mau cari apa di Google bro?"
    elif perintah in ["lu siapa", "kamu siapa", "siapa kamu"]:
        respon_ai = "Aku AI Aliy! Program asisten virtual berbasis web yang siap membantumu membuka aplikasi, melakukan hitungan, atau cari info di Google secara instan!"
    else:
        if "wait" in perintah or "tunggu" in perintah: respon_ai = "Oke bro! Aku tungguin nih, kalau sudah siap langsung ketik lagi ya!"
        elif "halo" in perintah or "hai" in perintah: respon_ai = "Halo juga! Ada hal menarik apa yang ingin kita lakukan atau bicarakan hari ini?"
        elif "kabar" in perintah: respon_ai = "Sistem kodenya berjalan dengan sangat optimal dan siap bekerja! Bagaimana dengan kabarmu sendiri?"
        elif "bosan" in perintah or "gabut" in perintah: respon_ai = "Wah lagi gabut ya bro? Coba ketik perintah 'buka roblox' atau 'buka ml' biar seru!"
        else: respon_ai = f"Hmm, aku belum memiliki jawaban untuk '{perintah_asli}'. Coba ketik 'cari {perintah_asli}' untuk cari di Google!"

    return respon_ai, redirect_url

input_user = st.text_input("Masukkan pesan atau perintah ke AI Aliy di sini:", key="user_input")
if input_user:
    jawaban, link_tujuan = proses_ai_web(input_user)
    st.session_state.riwayat_chat.insert(0, {"user": input_user, "ai": jawaban, "link": link_tujuan})

st.write("---")
st.subheader("📜 Tampilan Riwayat Obrolan:")
for chat in st.session_state.riwayat_chat:
    st.markdown(f"**👦 Kamu:** {chat['user']}")
    st.markdown(f"**🤖 AI Aliy:** {chat['ai']}")
    if chat['link']: st.link_button("👉 KLIK DI SINI UNTUK MEMBUKA LINK", chat['link'])
    st.write("---")
