> I was playing minecraft, and found this strange enchantment on the enchantment table. Can you figure out what it is? Wrap the flag in scriptCTF{}
> 
> Attachments
> enc.txt
> contenant : á’²â•Žãƒªá’·á“µâˆ·á”‘âŽ“â„¸ Ì£ â•Žá“­âŽ“âšãƒª

Indice du Standard Galactic Alphabet (SGA) pour plus tard (après recherche du langage utilisé dans Minecraft)
avec : 
```
s = "á’²â•Žãƒªá’·á“µâˆ·á”‘âŽ“â„¸ Ì£ â•Žá“­âŽ“âšãƒª" 
print(s.encode('latin1').decode('utf-8'))
```

Mais erreur car certains caractères (comme ’, l’apostrophe typographique \u2019) ne sont pas valides en Latin-1.

Le texte provient probablement d’un texte UTF-8 lu comme s’il était du Latin-1, donc pour le corriger, il faut faire l’opération inverse. Corriger Mojibake : utiliser .encode('utf-8').decode('latin1') avec le .txt (Python lit la chaîne comme des bytes en Latin-1 (c’est-à-dire : brut), donc les caractères ne sont pas déjà Unicode): 

```
with open("enc.txt", "r", encoding="latin1") as f:
    s = f.read()

msg = s.encode("latin1").decode("utf-8")
print(msg)
```

The result is : 
ᒲ╎リᒷᓵ∷ᔑ⎓ℸ ̣ ╎ᓭ⎓⚍リ

And dcode helps translating from the Standard Galactic Alphabet

<details>
<summary>Flag</summary>

`scriptCTF{MINECRAFTISFUN}`

</details>
