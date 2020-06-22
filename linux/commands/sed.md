## Sed (Stream Editor)


$ echo "unix is best, and unix is most best.Long Live unix" | sed 's/unix/linux':

    İlk unix gördüğü yerdeki kelimeyi linux yapar.
    s -> substraction
    / -> delimiter

$ echo "unix is best, and unix is most best.Long Live unix" | sed 's/unix/linux/2':

    İkinci gördüğü unix kelimesini linuxa çevirir.

$ echo "unix is best, and unix is most best.Long Live unix" | sed 's/unix/linux/g':

    Tüm gördüğü unix kelimelerini linuxa çevirir.

$ ls -la | sed '1d'

    İlk satırı siler.

$ ls -la | sed '1-5d'

    1-5 Arasındaki satırları siler.

$ ls -la | sed '$d'

    Son satırı siler.

$ ls -la | sed '/json/d'

    İçinde json geçen satırları siler.   

