# Домашнее задание к занятию 13 «Введение в мониторинг»

## Обязательные задания

1. Вас пригласили настроить мониторинг на проект. На онбординге вам рассказали, что проект представляет из себя платформу для вычислений с выдачей текстовых отчётов, которые сохраняются на диск. 
Взаимодействие с платформой осуществляется по протоколу http. Также вам отметили, что вычисления загружают ЦПУ. Какой минимальный набор метрик вы выведите в мониторинг и почему?

2. Менеджер продукта, посмотрев на ваши метрики, сказал, что ему непонятно, что такое RAM/inodes/CPUla. Также он сказал, что хочет понимать, насколько мы выполняем свои обязанности перед клиентами и какое качество обслуживания. Что вы можете ему предложить?

3. Вашей DevOps-команде в этом году не выделили финансирование на построение системы сбора логов. Разработчики, в свою очередь, хотят видеть все ошибки, которые выдают их приложения. Какое решение вы можете предпринять в этой ситуации, чтобы разработчики получали ошибки приложения?

3. Вы, как опытный SRE, сделали мониторинг, куда вывели отображения выполнения SLA = 99% по http-кодам ответов. 
Этот параметр вычисляется по формуле: summ_2xx_requests/summ_all_requests. Он не поднимается выше 70%, но при этом в вашей системе нет кодов ответа 5xx и 4xx. Где у вас ошибка?

### Ответы

1. Минимальный набор метрик будет состоять из:

  - Нагрузка на CPU, load average, кол-во wa
  - Использования памяти, swap
  - Состояние дисков, свободное пространство
  - Загруженность сетевого интерфейса
  - Общее кол-во http-запросов 
  - Кол-во 2хх, 4хх и 5хх ответов в % от обещго кол-ва запросов

2. В таком случае необходимо определить SLO и на основе этих целей задать основные SLI на основе которых будет осуществляться мониторинг соответствующих метрик.

3. Прежде всего в такой ситуации можно воспользоваться бесплатным тарифом Sentry. Дополнительно или как самостоятельное решение - написать собственную систему отслеживания ошибок на скриптах (bash, python) путем чтения логов. Для сбора и перенаправления логов можно использовать Vector. И в завершении настроить простую отправку результатов проверок разработчикам через postfix, telegram бота или иным способом.

4. Очевидно проблема в самой формуле. Дело в том, что помимо "ошибочных" 4хх, 5хх кодов и "успешных" 2хх, существуют "инфомационные" 1хх, а также коды "перенаправления" 3хх. Следовательно, в числитель необходимо добавить переменные summ_1xx_requests и summ_3xx_requests. Тогда формула будет корректной.

