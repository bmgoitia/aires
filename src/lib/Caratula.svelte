

<script>

    


</script>




<div id="lv_caratula">
    <div class="lv_caratulaWrapper">
      <div class="lv_caratulaBox">
          <div class="lv_caratulaItem lv_caratulaImg"></div>
          <div class="lv_caratulaItem lv_caratulaText">
              <div class="lv_jumbo">
                  <h3 class="lv_epigrafe">Crisis climática</h3>
                  <h1 class="lv_titular">¿Cuánto ha aumentado la demanda de aire acondicionado en tu provincia desde 1980?

                </h1>
                  <p class="lv_introText">Los hogares españoles necesitan el doble de climatización que hace 40 años</p>
                  <div class="lv_downArrows">
                      <span class="lv_downArrow"></span>
                      <span class="lv_downArrow"></span>
                  </div>
              </div>
              
          </div>
      </div>

    </div>

</div>

<div class="lv_separator"> <span></span></div>




<style>


 #lv_caratula{

}


.lv_caratulaWrapper{
    width: 100%;
    height: 100vh;
    position: relative;
}

.lv_caratulaBox{
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;

}

.lv_caratulaItem{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    position: absolute;

}

.lv_caratulaImg{
    z-index: 1;
    /* background-image: url('../assets/img/caratula/caratulaTruck3.png'); */
    background-image: url('../img/airess.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    
}


.lv_caratulaText{
    z-index: 2;
    align-items: flex-end;
    background: linear-gradient(#0000, #00000063, #000000);
    
 }

.lv_jumbo{
    margin-bottom: 20px;
    padding: 20px;
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background: linear-gradient(#0000, #00000063, #000000);

}


.lv_epigrafe{
    position: relative;
    color: #fde636;
    text-transform: uppercase;
    font-family: 'HMedium', serif;
    font-size: 16px;
    letter-spacing: 0.5px;
    line-height: 22px;
    text-decoration: underline;
    text-underline-offset: 8px;
}



.lv_titular{
    color: #fff;
    padding: 20px;
    font-size: 34px;
    font-family: 'HMedium', serif;
}

.lv_introText{
    display: none;
}

.lv_downArrows{

}


.lv_downArrow{
    display: block;
    width: 20px;
    height: 20px;
    transform: rotate(45deg);
    border-right: 2px solid white;
    border-bottom: 2px solid white;
    margin-top: -10px;
    opacity: .7;
}


.lv_separator{
    padding-bottom: 20px;
    width: 100%;
    height: 1px;
    text-align: center;
    background-color: #000000;
}

.lv_separator span{
    display: inline-block;
      width: 80%;
      height: 1px;
      border-top:1px dotted grey;
      
    
}




/* MEDIA QUERIES */

@media(min-width: 540px) {
    .lv_caratulaImg{
        background-image: url('../img/aires.jpg');
        background-repeat: no-repeat;
        background-size: cover;
        background-position: center;
    }


}




@media (min-width: 900px) {


    .lv_titular{
        font-size: 40px;

    }


    .lv_introText{
        display: block;
        color: #fff;
        font-family: 'HLight', serif;
        font-size: 1.2rem;
        margin-bottom: 20px;
        padding: 0 20px;
    }



    /* .lv_caratulaWrapper {
        height: calc(100vh - 50px) !important;
    } */

    .lv_caratulaBox{
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: row;
    
    }
    
    .lv_caratulaItem{
        position:initial;
    
    }
    
    
    
    .lv_caratulaText{
        background: #000;
        align-items: center;
        
    }

    .lv_titular{
        margin-top: 20px;
        margin-bottom: 20px;
    }



    .lv_introText{
        margin-bottom: 40px;
    }


    .lv_separator{
        display: none;
    }


    .lv_caratulaImg{
        background-image: url('../img/aires.jpg');
    }



}





@media (min-width: 1200px) {
    .lv_introText{
        padding: 10px 10%;
    }

    .lv_caratulaImg{
        background-image: url('../img/aires.jpg');
    }



}


@media (min-width: 1450px) {

}

    




</style>