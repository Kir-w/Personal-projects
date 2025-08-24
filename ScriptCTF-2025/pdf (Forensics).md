> so sad cause no flag in pdf :(
> 
> Attachments
> challenge.pdf

et dans le pdf il y a au milieu : 
“thx for comming
but no flag here :)”
avec au coin en bas à gauche un commentare "maybe look between stream and endstream"

Il faut ouvrir le fichier PDF avec un outil qui permet d'afficher son contenu brut, car les logiciels de lecture PDF classiques n'affichent pas les données cachées.

Un fichier PDF est un conteneur d'objets (texte, images, polices, etc.) qui sont organisés en flux de données, ou streams. Les données d'un stream sont délimitées par les mots-clés stream et endstream. Souvent, le contenu de ces streams est compressé (par exemple avec un filtre FlateDecode) ou encodé, mais il arrive que des données supplémentaires soient ajoutées en clair à des fins de dissimulation.

Ouvrir le fichier dans VS Code pour voir et inspecter le contenu (un fake flag).

Extraire tous les flux entre stream et endstream avec python : 
```
import re
import zlib


with open("challenge.pdf", "rb") as f:
    data = f.read()


# Chercher tous les blocs stream ... endstream
streams = re.findall(rb'stream(.*?)endstream', data, re.DOTALL)


# Décompresser chaque flux (si possible)
for i, s in enumerate(streams):
    s = s.strip()
    try:
        decompressed = zlib.decompress(s)
        print(f"\n--- Stream #{i} ---")
        print(decompressed.decode(errors='ignore'))  # ignore les erreurs de décodage
    except Exception as e:
        print(f"\n--- Stream #{i} --- [Failed to decompress] {e}")
```

Ce script va automatiquement lister les contenus réels cachés dans les flux PDF. Certains échouent à la décompression, c’est normal (pas tous les stream sont Flate).

<details>
<summary>Flag</summary>

`scriptCTF{pdf_s7r34m5_0v3r_7w17ch_5tr34ms}`

</details>

