<script>
    let elements = [
        {
            id: 1,
            emoji: '🐘',
            group: 1,
            selected: false,
            completed: false
        },
        {
            id: 2,
            emoji: '🦘',
            group: 1,
            selected: false,
            completed: false
        },
        {
            id: 3,
            emoji: '🦙',
            group: 1,
            selected: false,
            completed: false
        },
        {
            id: 4,
            emoji: '🐍',
            group: 1,
            selected: false,
            completed: false
        },
        {
            id: 5,
            emoji: '🫨',
            group: 2,
            selected: false,
            completed: false
        },
        {
            id: 6,
            emoji: '🫠',
            group: 2,
            selected: false,
            completed: false
        },
        {
            id: 7,
            emoji: '😳',
            group: 2,
            selected: false,
            completed: false
        },
        {
            id: 8,
            emoji: '😥',
            group: 2,
            selected: false,
            completed: false
        },
        {
            id: 9,
            emoji: '👩',
            group: 3,
            selected: false,
            completed: false
        },
        {
            id: 10,
            emoji: '💝',
            group: 3,
            selected: false,
            completed: false
        },
        {
            id: 11,
            emoji: '👩‍🍼',
            group: 3,
            selected: false,
            completed: false
        },
        {
            id: 12,
            emoji: '👒',
            group: 3,
            selected: false,
            completed: false
        },
        {
            id: 13,
            emoji: '🍊',
            group: 4,
            selected: false,
            completed: false
        },
        {
            id: 14,
            emoji: '🍊',
            group: 4,
            selected: false,
            completed: false
        },
        {
            id: 15,
            emoji: '🍊',
            group: 4,
            selected: false,
            completed: false
        },
        {
            id: 16,
            emoji: '🍊',
            group: 4,
            selected: false,
            completed: false
        }
            ];
    let answers = [{
        id: 1,
        title: 'animals',
        desc: '🐘🦘🦙🐍',
        color: '#a0c35a'
    },
    {
        id: 2,
        title: 'anxiety',
        desc: '🫨🫠😳😥',
        color: '#b0c4ef'
    },
    {
        id: 3,
        title: 'mother\'s day',
        desc: '👩💝👩‍🍼👒',
        color: '#ba81c5'
    },
    {
        id: 4,
        title: 'oranges',
        desc: '🍊🍊🍊🍊',
        color: '#f9df6d'
    },
];
    

    let canSubmit = false;
    let selected = [];
    let lastCompleteRow = 0;
    let mistakesLeft = 4;
    function handleSelect(id){
       
        var el = elements.find(f => f.id == id);
        
        if(!el.selected && selected.length < 4){
            el.selected = true;
            selected.push(el);
        }
        else{
            el.selected = false;
            selected = selected.filter(f => f.id !== el.id);
        }

        selected.length === 4 ? canSubmit = true : canSubmit = false;
        selected = selected;

        elements = elements;
    }
    function deselect(){
        elements.forEach(element => {
            element.selected = false;
        });
        elements = elements;
        selected = [];
    }
    shuffle();
    // Shuffle Elements
    function shuffle(){
        for(let i = elements.length - 1; i > 0; i--){
        const randomIndex = Math.floor(Math.random() * (i+1));
        [elements[randomIndex], elements[i]] = [elements[i], elements[randomIndex]];
        }
    elements = elements;
    }

    function submitGroup(){
        
        if(canSubmit){
            let success = false;
            let element = elements.find(f => f.id === selected[0].id);
            let sameGroup = selected.filter(f => f.group == element.group);
            if(sameGroup.length === 4){
                success = true;
            }
            if(success){

                // Move them to the front
                for (let i=0; i<4; i++){
                    var elIndex = elements.findIndex(e => e.id === sameGroup[i].id)
                    let element = elements[elIndex];
                    element.completed = true;
                    element.selected = false;
                    elements.splice(elIndex, 1);
                    elements.unshift(element);
                }
                selected = []
                elements = elements;
                // Remove them
                elements.splice(0,4);
                
                var answer = answers.find(f => f.id === sameGroup[0].group);
                // Add col-row answer
                var answerDiv = document.getElementById('answerDiv').cloneNode(true);
                answerDiv.style.display = "block";
                answerDiv.querySelector('h3').textContent = answer.title.toUpperCase();
                answerDiv.querySelector('p').textContent = answer.desc.toUpperCase();
                answerDiv.style.background = answer.color;

                document.getElementsByClassName("col-grid")[0].before(answerDiv);
               
                elements = elements;
                console.log(elements);
            }
            else{
                mistakesLeft -= 1;
            }
        }
    }
</script>
<div class="col" style='grid-column: 1/-1; display: none;' id="answerDiv"><h3></h3><p></p></div>

<div class="grid-container">
    <p>Create four groups of four, Emoji style!</p>
    <div class="grid" id="grid">
            {#each elements as col (col.id)}
            <div class="col col-grid" class:selected={col.selected}>
                <button on:click={() => {handleSelect(col.id)}}>{col.emoji}</button>
            </div>
            {/each}
    </div>
    <div id="mistake-counter">
        <p>Mistakes remaining: 
            {#each {length: mistakesLeft} as _, i}
            <i class="fa-solid fa-circle circle-icon fa-sm" style="color: #5a594e;"></i>
            {/each}
        </p>
    </div>
    <div class="action-items">
        <button on:click={shuffle}>Shuffle</button>
        <button on:click={deselect}>Deselect all</button>
        <button on:click={submitGroup} disabled={!canSubmit}>Submit</button>
    </div>
</div>


<style>
    .circle-icon{
        margin: 3px;
        
    }
    .action-items{
        display: flex;
        justify-content: center;
        
    }
    .action-items button{
        border-radius: 20px;
        padding: 10px 15px;
        background: #ffffff;
        margin-inline: 5px;
    }
    .action-items button:hover{
       cursor: pointer;
    }
    #mistake-counter{
        display: flex;
        justify-content: center;

    }
    .grid-container{
        display: block;
        justify-content: center;
        align-content: center;
        margin: 5rem auto;
        max-width: 500px;
        text-align: center;
        
    }
    .grid {
        display: grid;
        grid-template-columns: auto auto auto auto;
        border: 1pt solid white;
        -webkit-user-select: none; /* Safari */
        -ms-user-select: none; /* IE 10 and IE 11 */
        user-select: none; /* Standard syntax */
    }
    .col{
        border: 5pt solid white;
        padding: 5px;
        min-width: 100px;
        min-height: 100px;
        text-align: center;
        align-content: center;
        background: #efefe6;
        border-radius: 20px;
    }
    .col button{
        border: 0;
        font-size: 25pt;
        background: none;
        width: 100%;
        -webkit-user-select: none; /* Safari */
        -ms-user-select: none; /* IE 10 and IE 11 */
        user-select: none; /* Standard syntax */
    }
    .col button:hover{
        cursor: pointer;
    }
    .selected{
        background: #5a594e !important;
    }
    #answerDiv{
        font-size: 18pt;
    }
    #answerDiv h3, #answerDiv p{
        margin: 5px;
    }

    *{
        box-sizing: border-box;
    }
</style>