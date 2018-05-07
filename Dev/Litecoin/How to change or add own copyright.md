Копирайт
========

1 Файл COPYNG
-------------

Добавляем наш копирайт:

	Copyright (c) 2009-2014 Bitcoin Developers
	Copyright (c) 2011-2018 Litecoin Developers
	Copyright (c) 2018 Testcoin Developers

2 Файл configure.ac
-------------------

Изменям год в следующей строке:

	define(_COPYRIGHT_YEAR, 2018)

3 Файл src/clientversion.h
----------------------

Изменям год в следующей строке:

	#define COPYRIGHT_YEAR 2018

Строку:

	#define COPYRIGHT_STR "2009-" STRINGIZE(COPYRIGHT_YEAR) " The Bitcoin Core Developers

Приводим к следующему виду:

	#define COPYRIGHT_STR "2009-" STRINGIZE(COPYRIGHT_YEAR) " The Bitcoin Core Developers, 2014-" STRINGIZE(COPYRIGHT_YEAR) " The Litecoin Core Developers, " STRINGIZE(COPYRIGHT_YEAR) " The Testcoin Core Developers"

4 Файл src/qt/bitcoinstrings.cpp
--------------------------------

Добовляем строки после:

	QT_TRANSLATE_NOOP("bitcoin-core", "Copyright (C) 2009-%i The Bitcoin Core Developers"),

Чтобы стало так:

	QT_TRANSLATE_NOOP("bitcoin-core", "Copyright (C) 2009-%i The Bitcoin Core Developers"),
	QT_TRANSLATE_NOOP("bitcoin-core", "Copyright (C) 2011-%i The Litecoin Core Developers"),
	QT_TRANSLATE_NOOP("bitcoin-core", "Copyright (C) 2018 The Testcoin Core Developers"),


5 Файл src/qt/splashscreen.cpp
------------------------------

Добавляем копирайт testcoin:

	QString copyrightText1   = QChar(0xA9)+QString(" 2009-%1 ").arg(COPYRIGHT_YEAR) + QString(tr("The Bitcoin Core developers"));
    QString copyrightText2   = QChar(0xA9)+QString(" 2011-%1 ").arg(COPYRIGHT_YEAR) + QString(tr("The Litecoin Core developers"));
    QString copyrightText3   = QChar(0xA9)+QString(" 2018 ") + QString(tr("The Testcoin Core developers"));

Так же редактируем следующий код:

	pixPaint.drawText(pixmap.width()-paddingRightCopyright,paddingTop+paddingCopyrightTop,copyrightText1);
    pixPaint.drawText(pixmap.width()-paddingRightCopyright,paddingTop+paddingCopyrightTop+titleCopyrightVSpace,copyrightText2);
    pixPaint.drawText(pixmap.width()-paddingRightCopyright,paddingTop+paddingCopyrightTop+titleCopyrightVSpace+titleCopyrightVSpace,copyrightText3);


Файл init.cpp
-------------

Добавляем копирайт testcoin, и приводим к следующему виду:

	std::string LicenseInfo()
	{
	    return FormatParagraph(strprintf(_("Copyright (C) 2009-%i The Bitcoin Core Developers"), COPYRIGHT_YEAR)) + "\n" +
	           "\n" +
	           FormatParagraph(strprintf(_("Copyright (C) 2011-%i The Litecoin Core Developers"), COPYRIGHT_YEAR)) + "\n" +
	           "\n" +
	           FormatParagraph(strprintf(_("Copyright (C) %i The Testcoin Core Developers"), COPYRIGHT_YEAR)) + "\n" +
	           "\n" +
	           FormatParagraph(_("This is experimental software.")) + "\n" +
	           "\n" +
	           FormatParagraph(_("Distributed under the MIT software license, see the accompanying file COPYING or <http://www.opensource.org/licenses/mit-license.php>.")) + "\n" +
	           "\n" +
	           FormatParagraph(_("This product includes software developed by the OpenSSL Project for use in the OpenSSL Toolkit <https://www.openssl.org/> and cryptographic software written by Eric Young and UPnP software written by Thomas Bernard.")) +
	           "\n";
	}
