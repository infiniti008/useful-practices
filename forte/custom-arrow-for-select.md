```css
    select {
        background-image: 
            linear-gradient(225deg, transparent 50%, #fff 0),
            linear-gradient(315deg, #fff 50%, transparent 0),
            radial-gradient(#B09E70 100%, #B09E70 100%);
        background-position: calc(97.5%) 1.3em;
        background-size: 15px 15px;
        background-repeat: no-repeat;

        -moz-appearance: none;
        -webkit-appearance: none;
    }
    
    select::-ms-expand {
        display: none;
    }
    
    select:focus {
        background-image: 
            linear-gradient(45deg, transparent 50%, #fff 0),
            linear-gradient(135deg, #fff 50%, transparent 0),
            radial-gradient(#b09e70 100%, #b09e70 100%);
        background-position: calc(97.5%) .8em;
    }
```