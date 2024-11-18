<script>
	import { error } from "@sveltejs/kit";

    let characters = $state([]);



    let api = "https://dragonball-api.com/api/characters?limit=58";

    $effect(()=>{
        fetch(api)
            .then(res=>res.json())
            .then(data=>{
                console.log(data);
                characters = data.items;
                
            })
            .catch(error=>console.log("Error ",error))

    });

    let transformations = $state([]); //will carry array of transformations

    let show = $state("none");

    function handleClick(id)
    {
        
        fetch(`https://dragonball-api.com/api/characters/${id}`)
            .then(res=>res.json())
            .then(data=>{
                console.log(data.transformations);
                transformations = data.transformations;
                if(data.transformations.length>0){
                    show = "block";

                }
               
            })
            .catch(error=>console.log("Error ",error));

        
    }

</script>

<main>
<h1>Dragon Ball Fans</h1>
<br/>
<div style="display: {show};">
    <div class="chars">
        {#each transformations as transformation }
            <div id="transformations">
                <img loading="lazy" class="img-fluid" id={transformation.name} src={transformation.image} alt={transformation.name}>
                <p>{transformation.name}</p>
            </div>    
            {/each}
            
        </div>
        
    </div>
    <a style="display: {show};" href="#transformations" id="showTrans">
        <button id="showBtn">{null}</button>
    </a>
<div id="contents">
    
    {#await characters}
    <p>Loading ...</p>
    {:then}
        {#each characters as character,i}
            <div class="zoom" id="character-container">
                <a  onclick={handleClick(character.id)}  href={`#${character.id}`}>
                    
                    <img id={character.id} loading="lazy" class="img-fluid" src={character.image} alt={character.name +" image"}/>
                </a>
                <p >Name: {character.name}</p>
                {#if parseInt(character.ki) >0}
                <p>Ki: {character.ki}</p>
                    
                <p>Max Ki: {character.maxKi}</p>
                {/if}
            </div>
            {/each}
            {/await}
        </div>
        
            <br/>
        </main>

<style>
#showTrans{
    position: fixed;
    top: 50%;
    right: 3%;
}
h1{
    text-align: center;
    color: teal;
}


.zoom {
  padding: 50px;
  transition: transform .2s; /* Animation */

}

.zoom:hover {
  transform: scale(1.5); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */

}

@media (min-width:769px){
    #contents{
        display: grid;
        grid-template-columns: repeat(7,1fr);
        grid-gap: 5px;
    }
}
@media (max-width:769px){
    #contents{
        max-width: 16rem;
    }

}
p{
    font-size: large;
}

#transformations{
    max-width: 32rem;

       
}

#transformations img{
    width: 18rem;


}

.chars{
    display: grid;
    grid-template-columns: repeat(2,1fr);
    grid-gap: 5px;

}

#showBtn{
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: none;
    background-color: teal;
}


</style>