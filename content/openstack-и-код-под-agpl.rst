.. title: OpenStack и код под AGPL
.. slug: openstack-и-код-под-agpl
.. date: 2014-04-01 10:45:23
.. tags:
.. category:
.. link:
.. description:
.. type: text
.. author: Peter Lemenkov

**Это архивная статья**


Во время `одного из
обсуждений <https://thread.gmane.org/gmane.comp.cloud.openstack.devel/19930>`__
дизайна компонента `Marconi <https://wiki.openstack.org/wiki/Marconi>`__
(набор сервисов и API для организации шины данных в OpenStack), как само
собой разумеющийся факт было высказано, что зависимость Marconi от
NoSQL-базы MongoDB, `лицензированной под Affero
GPL <http://www.mongodb.org/about/licensing/>`__, это проблема. Ряд
участников обсуждения `попросили аргументированно объяснить
ситуацию <https://thread.gmane.org/gmane.comp.cloud.openstack.devel/19930/focus=20053>`__.

На просьбу откликнулся известный юрист, `Gil
Yehuda <https://twitter.com/gyehuda>`__.

`В своем
письме <https://thread.gmane.org/gmane.comp.cloud.openstack.devel/19930/focus=20073>`__,
которое он попросил переслать в список рассылки OpenStack, Gil
настоятельно советует не добавлять зависимость от AGPL-компонентов,
особенно от MongoDB, которая, по его словам, лицензирована в очень
странной манере. Использование MongoDB приводит к непреднамеренным
нарушениям лицензионного соглашения (мечта юриста, указывает Gil).

Несмотря на то, что драйверы для MongoDB лицензированы под `ASL
2.0 <https://www.apache.org/licenses/LICENSE-2.0.html>`__, он приводит
подробное объяснение, почему это не может считаться достаточными
гарантиями безопасности. AGPL, покрывающая ядро MongoDB, может быть
прочитана таким образом, что драйверы для базы также попадают под ее
условия. Gil подчеркивает, что это его личная трактовка, и ситуация не
была проверена в суде США (американские законы, это именно те законы, по
которым живет глобальная айти-индустрия), но риски слишком велики.

Конечно, MongoDB неоднократно заявляла, что не собирается идти в суд с
их лицензией, но как правило, наличие пунктов в юридических соглашениях
подразумевает, что сторона, предлагающая их, имеет в виду именно то, что
написано, иначе их бы не было в соглашениях.

OpenStack, это продукт, развиваемый огромным количеством компаний,
продолжает Gil, и использование MongoDB просто приводит к сокращению
возможностей для коммерческого использования проекта. Минусы
использования MongoDB перевешивают технические плюсы.

Если вы используете MongoDB, то это ваше дело, но непозволительно
превращать пользователей OpenStack в ничего не подозревающих
пользователей MongoDB (и попадающих под действие AGPL). Наоборот, стоит
отделить и изолировать MongoDB от проекта полностью, рекомендует Gil.

