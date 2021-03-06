.. title: Первые бенчмарки kdbus
.. slug: Первые-бенчмарки-kdbus
.. date: 2014-01-19 01:20:25
.. tags: kdbus, systemd, canonical
.. category:
.. link:
.. description:
.. type: text
.. author: Peter Lemenkov

Lennart Poettering в рассылке
`dbus@lists.freedesktop.org <http://lists.freedesktop.org/mailman/listinfo/dbus>`__
к конце прошлого года анонсировал `общую готовность
kdbus <https://thread.gmane.org/gmane.comp.freedesktop.dbus/15499>`__ и
предложил начать процесс портирования приложений с библиотеки dbus1 на
kdbus. Как обычно, предложение вызвало волну обсуждений - почему kbdus
требует systemd? что будет с D-Bus под Windoz и Mac OS X? будет ли
включена поддержка kdbus в GNOME и KDE? почему GVariant как
wire-protocol формат? и т.п. Lennart постарался на все ответить, и, хотя
было видно, что часть участников обсуждения из Canonical недовольны
ситуацией, в целом обсуждение было конструктивно и полезно.

Из особенно полезного, инженер Samsung, `Lukasz
Skalski <https://github.com/lukasz-skalski>`__, который работает над
`включением kdbus в
glib <https://github.com/lukasz-skalski/glib-kdbus>`__, сообщил, что
`провел серию бенчмарков kdbus и
D-Bus <https://thread.gmane.org/gmane.comp.freedesktop.dbus/15499/focus=15670>`__.

Из тестов складывается очень интересная картина (чем меньше, тем лучше):

.. image:: http://peter.fedorapeople.org/stuff/kdbus_benchmark.png
   :align: center

+----------------+---------------------------+----------------------+
| Message size   | Elapsed time              | Elapsed time         |
|                | GLIB WITH KDBUS SUPPORT   | GLIB + DBUS DAEMON   |
+================+===========================+======================+
| 1000 x 4kB     | 1.351737 s                | 1.870417 s           |
+----------------+---------------------------+----------------------+
| 1000 x 8kB     | 1.349266 s                | 1.857693 s           |
+----------------+---------------------------+----------------------+
| 1000 x 16kB    | 1.383427 s                | 2.219304 s           |
+----------------+---------------------------+----------------------+
| 1000 x 32kB    | 1.358608 s                | 2.542795 s           |
+----------------+---------------------------+----------------------+
| 1000 x 64kB    | 1.878409 s                | 3.062035 s           |
+----------------+---------------------------+----------------------+
| 1000 x 128kB   | 2.265555 s                | 4.043454 s           |
+----------------+---------------------------+----------------------+
| 1000 x 256kB   | 3.112191 s                | 6.657750 s           |
+----------------+---------------------------+----------------------+
| 1000 x 512kB   | 3.383699 s                | 11.400224 s          |
+----------------+---------------------------+----------------------+
| 1000 x 1MB     | 4.703610 s                | 19.041988 s          |
+----------------+---------------------------+----------------------+

Видно, что квадратичного роста времени отклика в зависимости от размера
сообщения *почти* удалось избежать (с превышением 32 килобайт, что
является текущим практическим лимитом для приложений, использующих
D-Bus, отклик начинает медленно и почти линейно расти). Таким образом,
становится возможным начинать осторожно использовать kdbus, как
полноценную шину данных, скрепляя компоненты системы `не
пайпами </content/Предложены-радикальные-изменения-в-работу-unix-pipes>`__,
линкуя их с букетами библиотек, или как-то еще, а с помощью message
passing (типизованными сообщениями) через kdbus. Это теоретически
повысит надежность всей системы, и у нас постепенно появляется
возможность сильнее изолировать runtime-компоненты. Практически
`Unix-way Reloaded <https://ru.wikipedia.org/wiki/Философия_UNIX>`__!
