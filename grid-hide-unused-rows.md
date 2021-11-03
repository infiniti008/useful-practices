- Определение количества строк для отображения и удаление неиспользуемых если пришло айтемов меньще чем нужно для заполнения строк
- Рамки для элементов в сетке с помощью grid-gap


```css
    .products-list-content {
        display: grid;
        grid-gap: 1px; // Устанавливаем отступ между элементами
        grid-template-columns: repeat(2, 50%);
        padding: 1px 1px; // Паддинг для того чтобы не пропали внешние рамки

        @media screen and (min-width: $--screen-sm-min) {
            grid-template-columns: repeat(3, 1fr); // Три колонки
            grid-template-rows: 1fr 1fr minmax(0, min-content) minmax(0, min-content); //Определяем количество строк, minmax(0, min-content) задает возможность делать строку необязательной. Это важный параметр для сокрытия строк для которых не пришел контент!!!
            overflow: hidden; // Скрываем ненужные строки
            grid-auto-rows: 0; // Определяем высоту для ненужных строк
        }

        @media screen and (min-width: $--screen-md-min) {
            grid-template-columns: repeat(4, 1fr); // Четыре колонки
        }

        @media screen and (min-width: $--screen-lg-min) {
            grid-template-columns: repeat(5, 1fr); // Пять колонок
            grid-template-rows: 1fr 1fr minmax(0, min-content); // Три строки, 3 необязательная
        }
    }
```