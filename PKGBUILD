# Contributor: spider-mario <spidermario@free.fr>
pkgname=macaque-darcs
pkgver=20120416
pkgrel=1
pkgdesc="DSL for OCaml which produces SQL queries from a comprehension syntax"
arch=('any')
url="http://ocsigen.org/macaque/"
license=('custom:LGPL')
depends=('ocaml' 'pgocaml')
makedepends=('darcs' 'ocaml-findlib')
options=('!strip' '!makeflags')

_darcstrunk="http://forge.ocamlcore.org/anonscm/darcs/macaque"
_darcsmod="macaque"

build() {
  cd "$srcdir"

  msg "Checking for previous build"

  if [[ -d $_darcsmod/_darcs ]]
  then
    msg "Retrieving missing patches"
    cd $_darcsmod
    darcs pull -a $_darcstrunk/$_darcsmod
  else
    msg "Retrieving complete sources"
    darcs get --lazy --set-scripts-executable $_darcstrunk/$_darcsmod
    cd $_darcsmod
  fi

  rm -rf "$srcdir/$_darcsmod-build"
  cp -r "$srcdir/$_darcsmod" "$srcdir/$_darcsmod-build"
  cd "$srcdir/$_darcsmod-build/src"

  msg "Starting build"

  make
}

package() {
  cd "$srcdir/$_darcsmod-build/src"
  install --directory "$pkgdir/usr/lib/ocaml/"
  install --directory "$pkgdir/usr/share/licenses/$pkgname/"
  install --mode=644 ../LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  ocamlfind install -destdir "$pkgdir/usr/lib/ocaml/" macaque META \
	_build/sql_base.cmi _build/sql_types.cmi _build/sql_parsers.cmi \
	_build/sql_printers.cmi _build/sql_builders.cmi _build/sql_public.cmi \
	_build/inner_sql.cmi \
	_build/sql.cmi _build/check.cmi _build/query.cmi \
	_build/macaque.cma \
	_build/pa_macaque.cmo _build/pa_bananas.cmo
}
