> Emojis everywhere! Is it a joke? Or something is hiding behind it.
> 
> Attachments
> 
> out.txt
> 
> https://storage.googleapis.com/scriptctf_challenges/Misc/emoji/out.txt 
> avec ðŸ³ðŸ£ðŸ²ðŸ©ðŸ°ðŸ´ðŸƒðŸ”ðŸ†ðŸ»ðŸ€³ðŸ­ðŸ€°ðŸªðŸ€±ðŸŸðŸ€³ðŸ®ðŸ£ðŸ€°ðŸ¤ðŸ€±ðŸ®ðŸ§ðŸŸðŸ€±ðŸ³ðŸŸðŸ·ðŸ€³ðŸ€±ðŸ²ðŸ¤ðŸŸðŸ€´ðŸ®ðŸ¤ðŸŸðŸ¦ðŸµðŸ®ðŸ€¡ðŸ€±ðŸ¥ðŸ€´ðŸ€¶ðŸ¤ðŸ½

Séquence d’emojis correspondante : 

🁳 🁣 🁲 🁩 🁰 🁴 🁃 🁔 🁆 🁻 🀳 🁭 🀰 🁪 🀱 🁟 🀳 🁮 🁣 🀰 🁤 🀱 🁮 🁧 🁟 🀱 🁳 🁟 🁷 🀳 🀱 🁲 🁤 🁟 🀴 🁮 🁤 🁟 🁦 🁵 🁮 🀡 🀱 🁥 🀴 🀶 🁤 🁽

Chaque tuile correspond à un code Unicode du bloc U+1F000 à U+1F0FF.
On transforme chaque emoji en son code Unicode, par exemple :
🁳 : U+1F073
🁣 : U+1F063
🁲 : U+1F072
🁩 : U+1F069
🁰 : U+1F070
🁴 : U+1F074
🁃 : U+1F043
🁔 : U+1F054
🁆 : U+1F046
🁻 : U+1F07B
🀳 : U+1F033
🁭 : U+1F06D
🀰 : U+1F030
🁪 : U+1F06A
🀱 : U+1F031
🁟 : U+1F05F
🀳 : U+1F033
🁮 : U+1F06E
🁣 : U+1F063
🀰 : U+1F030
🁤 : U+1F064
🀱 : U+1F031
🁮 : U+1F06E
🁧 : U+1F067
🁟 : U+1F05F
🀱 : U+1F031
🁳 : U+1F073
🁟 : U+1F05F
🁷 : U+1F077
🀳 : U+1F033
🀱 : U+1F031
🁲 : U+1F072
🁤 : U+1F064
🁟 : U+1F05F
🀴 : U+1F034
🁮 : U+1F06E
🁤 : U+1F064
🁟 : U+1F05F
🁦 : U+1F066
🁵 : U+1F075
🁮 : U+1F06E
🀡 : U+1F021
🀱 : U+1F031
🁥 : U+1F065
🀴 : U+1F034
🀶 : U+1F036
🁤 : U+1F064
🁽 : U+1F07D

Chaque emoji correspond à un code Unicode qui peut être converti en une valeur hexadécimale.

Script Python pour convertir les emojis en codes Unicode hexadécimaux et les analyser :

```
# Liste des emojis (symbole Unicode)
emojis = [
    "🁳", "🁣", "🁲", "🁩", "🁰", "🁴", "🁃", "🁔", "🁆", "🁻", "🀳", "🁭", 
    "🀰", "🁪", "🀱", "🁟", "🀳", "🁮", "🁣", "🀰", "🁤", "🀱", "🁮", "🁧", 
    "🁟", "🀱", "🁳", "🁟", "🁷", "🀳", "🀱", "🁲", "🁤", "🁟", "🀴", "🁮", 
    "🁤", "🁟", "🁦", "🁵", "🁮", "🀡", "🀱", "🁥", "🀴", "🀶", "🁤", "🁽"]

# Convertir chaque emoji en son code Unicode (hexadécimal)
unicode_codes = [hex(ord(emoji)) for emoji in emojis]

# Afficher les codes Unicode hexadécimaux
for code in unicode_codes:
    print(code)
```

avec : ord(emoji) : Cette fonction récupère le code point Unicode d'un caractère (par exemple, 🁳).
hex() : Cette fonction convertit le code point Unicode en une valeur hexadécimale.

on obtient 0x1f073 0x1f063 0x1f072 0x1f069 0x1f070 0x1f074 0x1f043 0x1f054 0x1f046 0x1f07b 0x1f033 0x1f06d 0x1f030 0x1f06a 0x1f031 0x1f05f 0x1f033 0x1f06e 0x1f063 0x1f030 0x1f064 0x1f031 0x1f06e 0x1f067 0x1f05f 0x1f031 0x1f073 0x1f05f 0x1f077 0x1f033 0x1f031 0x1f072 0x1f064 0x1f05f 0x1f034 0x1f06e 0x1f064 0x1f05f 0x1f066 0x1f075 0x1f06e 0x1f021 0x1f031 0x1f065 0x1f034 0x1f036 0x1f064 0x1f07d

On remarque que tous les codes sont du type 0x1f0XX. Si on retire 0x1f000 à chaque valeur, on obtient un numéro dans la plage ASCII. Par exemple, 0x1f073 - 0x1f000 = 0x73 = 115 (soit le caractère 's').

-> Décodage en ASCII
Pour chaque emoji, on effectue :
code_ascii = code_unicode - 0x1f000
On transforme ce nombre en caractère ASCII : chr(code_ascii)
On ignore les valeurs qui ne donnent pas un caractère imprimable (hors 32-126).
En appliquant ce procédé à toute la liste, on obtient :


<details>
<summary>Flag</summary>

`scriptCTF{3m0j1_3nc0d1ng_1s_w31rd_4nd_fun!1e46d}`

</details>
