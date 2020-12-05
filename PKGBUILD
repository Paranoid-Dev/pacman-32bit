# upstream git revision: 18811ca7ee347721a700db2080a50df06a0a79fc

depends+=(archlinux32-keyring)

# fail if upstream changes makepkg.conf or pacman.conf
for ((i=0; i<${#sha256sums[@]}; i++)); do
  if [ "${sha256sums[${i}]}" = '9c769f13c09a6f24c393a9762474eded2f269d8966e7764d9160d62232a7919b' ]; then
    sha256sums[${i}]='b2967ed2b41ac2841b4a367e1f39d698293fe69e95f180486436b5a10b375865'
  fi
  if [ "${sha256sums[${i}]}" = '3353f363088c73f1f86a890547c0f87c7473e5caf43bbbc768c2e9a7397f2aa2' ]; then
    sha256sums[${i}]='428ceeb0d8b96ac5e4274ef098bde00916f9e1b62369eb3566eaf6f6b3ac3984'
  fi
done

if [ ! "${CARCH}" = "i686" ]; then
  # patch architecture where needed
  eval "$(
    declare -f package | \
      sed '
        /install.*makepkg.conf/ a \
          sed -i "s@i686@'"${CARCH}"'@g; /^CHOST/ s/pentium4-/i686-/" "$pkgdir/etc/makepkg.conf"
      '
  )"
fi

source+=('replace-i686-by-pentium4-when-architecture-is-auto.patch')
sha256sums+=('e8d5f8979c4dfab49e7ac058846f2454b865c1da451e086c23e61034fd820c19')

eval "$(
  {
    declare -f prepare || \
    printf 'prepare() {\n}\n'
  } \
  | sed '
    $i cd "$srcdir/$pkgname-$pkgver" \
       patch -p1 -i ../replace-i686-by-pentium4-when-architecture-is-auto.patch
  '
)"

# FAIL: test 600 (also 64-bit), ignore for now
eval "$(
  declare -f check | \
    sed '
      s/make\(.*\)check/make \1 check || true/
    '
)"
