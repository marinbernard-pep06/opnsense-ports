PORTNAME=	pdf-tools
DISTVERSIONPREFIX=	v
DISTVERSION=	0.91-63
DISTVERSIONSUFFIX=	-gaec8ecd
CATEGORIES=	print elisp
PKGNAMESUFFIX=	${EMACS_PKGNAMESUFFIX}

MAINTAINER=	jrm@FreeBSD.org
COMMENT=	Emacs support library for PDF files
WWW=		https://github.com/vedang/pdf-tools

LICENSE=	GPLv3+
LICENSE_FILE=	${WRKSRC}/COPYING

BROKEN_FreeBSD_12_powerpc64=	fails to configure on powerpc64 elfv1: cannot find necessary poppler-private header (see README.org)

BUILD_DEPENDS=	cask${EMACS_PKGNAMESUFFIX}>0:devel/cask@${EMACS_FLAVOR} \
		tablist${EMACS_PKGNAMESUFFIX}>=0.70.4:devel/tablist@${EMACS_FLAVOR}
LIB_DEPENDS=	libpng.so:graphics/png \
		libpoppler-glib.so:graphics/poppler-glib \
		libpoppler.so:graphics/poppler
RUN_DEPENDS=	tablist${EMACS_PKGNAMESUFFIX}>=0.70:devel/tablist@${EMACS_FLAVOR}

USES=		autoreconf:build emacs gettext-runtime gmake gnome magick:7
USE_GITHUB=	yes
GH_ACCOUNT=	vedang
USE_GNOME=	cairo glib20

EMACS_FLAVORS_EXCLUDE=	devel_nox nox

CONFLICTS_INSTALL=	pdf-tools pdf-tools-emacs_* # bin/epdfinfo

do-build:
	cd ${WRKSRC}/server && ./autogen.sh && ./configure -q && make
	${EMACS_CMD} -q --batch -L ${WRKSRC}/lisp -f batch-byte-compile \
		${WRKSRC}/lisp/*.el
.if ${FLAVOR:U} == devel_full
	 ${EMACS_CMD} -q --batch -f loaddefs-generate-batch \
		${WRKSRC}/lisp/${PORTNAME}-loaddefs.el ${WRKSRC}/lisp
.else
	${EMACS_CMD} -q --batch \
	--eval "(setq generated-autoload-file \
		\"${WRKSRC}/lisp/${PORTNAME}-loaddefs.el\" \
		backup-inhibited t)" \
	-f batch-update-autoloads ${WRKSRC}/lisp
.endif

do-install:
	@${MKDIR} ${STAGEDIR}${PREFIX}/${EMACS_SITE_LISPDIR}/${PORTNAME}
	${INSTALL_PROGRAM} ${WRKSRC}/server/epdfinfo ${STAGEDIR}${PREFIX}/bin
	${INSTALL_DATA} ${WRKSRC}/lisp/* ${FILESDIR}/${PORTNAME}-init.el \
		${STAGEDIR}${PREFIX}/${EMACS_SITE_LISPDIR}/${PORTNAME}

.include <bsd.port.mk>
