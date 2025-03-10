PORTNAME=	magit
DISTVERSIONPREFIX=	v
DISTVERSION=	3.3.0-443
DISTVERSIONSUFFIX=	-gcfe5a126
CATEGORIES=	devel elisp
PKGNAMESUFFIX=	-devel${EMACS_PKGNAMESUFFIX}

MAINTAINER=	yasu@FreeBSD.org
COMMENT=	Interface to Git for Emacs (Development Version)
WWW=		https://magit.vc/

LICENSE=	GPLv3+
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	compat.el${EMACS_PKGNAMESUFFIX}>=28.1.1.0:misc/compat.el@${EMACS_FLAVOR} \
		dash.el${EMACS_PKGNAMESUFFIX}>=2.19.1:devel/dash.el@${EMACS_FLAVOR} \
		transient-devel${EMACS_PKGNAMESUFFIX}>=0.3.7:devel/transient-devel@${EMACS_FLAVOR} \
		with-editor-devel${EMACS_PKGNAMESUFFIX}>=3.0.5:editors/with-editor-devel@${EMACS_FLAVOR}
RUN_DEPENDS=	compat.el${EMACS_PKGNAMESUFFIX}>=28.1.1.0:misc/compat.el@${EMACS_FLAVOR} \
		dash.el${EMACS_PKGNAMESUFFIX}>=2.19.1:devel/dash.el@${EMACS_FLAVOR} \
		transient-devel${EMACS_PKGNAMESUFFIX}>=0.3.7:devel/transient-devel@${EMACS_FLAVOR} \
		with-editor-devel${EMACS_PKGNAMESUFFIX}>=3.0.5:editors/with-editor-devel@${EMACS_FLAVOR}

USES=		emacs gmake makeinfo
USE_GITHUB=	yes

MAKE_ENV+=	docdir=${DOCSDIR} \
		infodir=${PREFIX}/${INFO_PATH} \
		INSTALL_INFO=install-info \
		lispdir=${PREFIX}/${ELISPDIR} \
		LOAD_PATH="-L ${WRKSRC}/lisp -L ${PREFIX}/${EMACS_SITE_LISPDIR} -L ${PREFIX}/${EMACS_VERSION_SITE_LISPDIR} -L ${PREFIX}/${EMACS_VERSION_SITE_LISPDIR}/compat -L ${PREFIX}/${EMACS_VERSION_SITE_LISPDIR}/ghub" \
		VERSION=${PORTVERSION}
# Parallel build fails if Emacs is built with native compilation enabled
MAKE_JOBS_UNSAFE=	yes
ALL_TARGET=	lisp info
INSTALL_TARGET=	install-lisp install-info

CONFLICTS_INSTALL=	magit-emacs*

NO_ARCH=	yes

INFO=		magit magit-section
PLIST_SUB+=	ELISPDIR=${ELISPDIR}
PORTDOCS=	*.html *.md RelNotes/*

OPTIONS_DEFINE=	DOCS LIBEGIT2 PDF
OPTIONS_SUB=	yes

LIBEGIT2_DESC=	Use libegit2 as backend (EXPERIMENTAL)
PDF_DESC=	Build and install PDF document

DOCS_BUILD_DEPENDS=		gsed:textproc/gsed
DOCS_ALL_TARGET=		html
DOCS_BINARY_ALIAS=		sed=gsed
LIBEGIT2_BUILD_DEPENDS=		libegit2${EMACS_PKGNAMESUFFIX}>=g20200515:devel/libegit2@${EMACS_FLAVOR}
LIBEGIT2_BUILD_DEPENDS_OFF=	git:devel/git
LIBEGIT2_RUN_DEPENDS=		libegit2${EMACS_PKGNAMESUFFIX}>=g20200515:devel/libegit2@${EMACS_FLAVOR}
LIBEGIT2_RUN_DEPENDS_OFF=	git:devel/git
LIBEGIT2_VARS=			MAKE_ENV+=BUILD_MAGIT_LIBGIT=true
PDF_BUILD_DEPENDS=		texi2pdf:print/texinfo
PDF_USE=			TEX=dvipdfmx:build
PDF_ALL_TARGET=			pdf

ELISPDIR=	${EMACS_VERSION_SITE_LISPDIR}/${PORTNAME}

do-install-DOCS-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}/RelNotes
	${INSTALL_DATA} ${WRKSRC}/docs/*.md ${STAGEDIR}${DOCSDIR}
	${COPYTREE_SHARE} ${WRKSRC}/docs/RelNotes ${STAGEDIR}${DOCSDIR}/RelNotes

do-install-PDF-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
	${INSTALL_DATA} ${WRKSRC}/docs/*.pdf ${STAGEDIR}${DOCSDIR}

.include <bsd.port.mk>
