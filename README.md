# 📺 Magyar TV (MTVA) IPTV Generátor + Visszanézhető műsorok (VOD)

Mivel nem találtam jelenleg jól működő megoldást a magyar adók házi streamelésére, készítettem egy kis scriptet, amit Jellyfin média szerverbe könnyedén lehet integrálni.

Ez a projekt egy pehelysúlyú, dockerizált python alkalmazás, ami egy folyamatosan frissülő, élő M3U lejátszási listát generál a magyar állami televíziócsatornákhoz. 

A hivatalos MTVA streamek biztonsági tokeneket használnak, amik percek alatt lejárnak. Ez a kód megkerüli a lejáró linkek problémáját, így a csatornák stabilan és megszakítás nélkül nézhetők bármilyen IPTV lejátszóban vagy médiaszerverben (Jellyfin, Emby, Plex, VLC).

## 📺 Támogatott Csatornák
* M1 HD
* M2 HD
* M4 Sport HD
* M4 Sport+ HD
* M5 HD
* Duna HD
* Duna World HD


## 🚀 Telepítés (Docker)

A telepítéshez Docker és Docker Compose szükséges.

1. Klónozd a tárolót:
```
   git clone https://github.com/penguinpit/magyartv.git
```
```
   cd magyartv
```
2.
  ```
   docker compose up -d --build
  ```
A generátor mostantól a háttérben fut.
Az M3U lejátszási listádat a következő linken éred el:
```
   http://<A_SZERVERED_IP_CIME>:8000/m3u
```
## 🚀 Jellyfin integráció

. A Tuner hozzáadása
Lépj a Jellyfin Vezérlőpult -> Élő TV menüjébe.

Kattints a Tuner eszközök alatt a + gombra.

Típus: M3U Tuner.

Fájl vagy webcím:
```
http://<A_SZERVERED_IP_CIME>:8000/m3u
```
```
Mentés, és műsorujság frissítése gombra katt
```
## 👉Egyéb megjegyzések👈
A legtöbb beállítás hagyható defaulton, viszont a fMP4 átkódoló konténer engedélyezése résznél érdemes kivenni a pipát, ha bent volt, ellenkező esetben a terhelés jelentősen megugrik. A többi maradhat.
Böngészős lejátszás nem javasolt, mivel a böngészők sajátossága miatt gond lehet a hang lejátszásával. Javasolt Android TV vagy Desktop alkalmazás használata a lejátszáshoz.
Ha  valami nem működik, érdemes a műsorújság frissítése gombra kattintani, vagy a médiatár beolvasására. (sokszor megoldja a problémát)

<img width="1091" height="793" alt="image" src="https://github.com/user-attachments/assets/29eb5983-38dc-4947-a9aa-8a04d52b1647" />

## 🎬 Visszanézhető műsorok (VOD) beállítása

1. Menj be a jellyfin mappájába. 
Keresd meg a docker-compose.yml fájlt. Azon belül pedig a volumes: részt és add hozzá a saját útvonalad. Figyelj a szóközökre! Valahogy így kell kinéznie:
```
    volumes:
      - /ELERESI_UTVONALAM/magyartv/vod_output:/media/visszanezo
```
Az ELERESI_UTVONALAM részt írd át a sajátodra, a többi maradjon!

2. Jellyfin -> Vezérlőpult -> Könyvtárak (Libraries).

```+ Könyvtár hozzáadása.```

Tartalom típusa: Sorozatok (TV Shows).

Megjelenítendő név: Pl. Visszanézhető TV.

3. Kattints a Mappák melletti``` + ```jelre.

Jellyfin felületén valahogy így fog megjelenni ```/media/visszanezo```

FONTOS: A Könyvtár beállításainál görgess lejjebb, és pipáld be:

✅ Valós idejű figyelés engedélyezése (Enable Realtime Monitoring)

(Így amint a script letölt egy új részt, a Jellyfin azonnal észreveszi).

Okézd le, és várj pár percet, amíg a Jellyfin leszedi a képeket.(Ha nem tudta leszedni a képeket, saját magad is szerkesztheted, hozzáadhatod)
## 👉Kiegészítés👈
Itt adhatsz hozzá újabb műsorokat, illetve törölhetsz is.
config/shows.txt
```
Kékfény,https://mediaklikk.hu/musor/kekfeny/
Híradó,https://mediaklikk.hu/musor/hirado/
Család-barát,https://mediaklikk.hu/musor/csalad-barat/
Fókuszban,https://mediaklikk.hu/musor/fokuszban/
```
Ez alapján olvassa be a script a műsorokat. A script 12 óránként fut le.
 
<img width="2505" height="1278" alt="image" src="https://github.com/user-attachments/assets/c6acf858-7163-4b3a-b1c0-a030ec0a3757" />

