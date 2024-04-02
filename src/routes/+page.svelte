<script>
    //Sette opp variablar
    let oppdrag = [];
    let id;
    let Dato = null;
    let Namn = null;
    let Platform = null;
    let Problem = null;
    let Feilmelding;
    let FeilmeldingStatus = false;
    let count = {
        "Windows 10": 0,
        "Windows 11": 0,
        "macOS": 0,
        "iOS": 0,
        "Android": 0,
        "Linux": 0,
        "Annet": 0,
    };
    //Funksjon for å telle kvar platform
    function calculateCount() {
        //sette tellingen til 0
        count = {
        "Windows 10": 0,
        "Windows 11": 0,
        "macOS": 0,
        "iOS": 0,
        "Android": 0,
        "Linux": 0,
        "Annet": 0, 
        }
        //Telle opp alle platformar
        oppdrag.forEach((item) => {
            if (count.hasOwnProperty(item.Platform)) {
                count[item.Platform] += 1;
            }
        });
    }
    

    // Funksjon for å legge til oppdrag.
    function leggTil() {
        //Sjekke om det er data i kvart felt, viss det er det så fortsetter den. Viss det mangler, så går den til else
        if (Dato  != null && Namn != null && Platform  != null && Problem != null){
            FeilmeldingStatus = false
        //Sjølve innsetinga
        oppdrag.push( 
            {
            "id": Math.round(Math.random() * 1000000),
            "Dato": Dato,
            "Namn": Namn,
            "Platform": Platform,
            "Problem": Problem
            }
        );
        //Sortere arrayen etter dato
        oppdrag = oppdrag.sort((a, b) => {
            return new Date(a.Dato) - new Date(b.Dato)
        })
        //Nullstille felta/variablane slik at felta blir tømt og slik at sjekkinga av felt fungerer rett.
        Dato = null;
        Namn = null;
        Platform = null;
        Problem = null;
        calculateCount()
        //Kode som viser feilmelding viss eit eller fleire felt er tomme. Den sjekker etter kva fellt som er tomme, og legger til i feilmeldinga kva det er som mangler.
        } else {
            FeilmeldingStatus = true
            Feilmelding = "Mangler " + " " +
            (Dato != null ? "" :  "dato,") + " " +
            (Namn != null ? "" :  "namn,") + " " +
            (Platform != null ? "" :  "platform,") + " " +
            (Problem != null ? "" :  "problem")
        }  
    }

    //Funskjon for å slette eit oppdrag
    function fjern (id) {
        oppdrag = oppdrag.filter(t => t.id != id)
        calculateCount()
        FeilmeldingStatus = false
    }

    //Funksjon for å fjerne alle oppdrag
    function fjernAlt() {
        oppdrag = []
        calculateCount()
        FeilmeldingStatus = false
    }


</script>
<table>
    <tr>
        <th>Dato</th>
        <th>Namn</th>
        <th>Platform</th>
        <th>Problem</th>
        <!--Knapp for å fjerne alle oppdragene-->
        <th><button on:click={fjernAlt}>Slett alt</button></th>
    </tr>
    <!--Denne koden ser etter oppdrag, og for kvart oppdrag den finner så lager den ein til rad i tabellen--> 
    {#each oppdrag as {id, Dato, Namn, Platform, Problem} }
        <tr>
            <td>{new Date(Dato).getDate()}/{new Date(Dato).getMonth()}/{new Date(Dato).getFullYear()}</td>
            <td>{Namn}</td>
            <td>{Platform}</td>
            <td class="textarea">{Problem}</td>
            <!--Knapp for å fjerne innleget frå tabellen-->
            <td><button on:click={() => fjern(id)}>Fjern</button></td>
        </tr>
    {/each}
    <tr class="input">
        <td>
            <input name="Dato" bind:value={Dato} type="date">
        </td>   
        <td>
            <input name="Namn" bind:value={Namn} type="text">
        </td>
        <td>
            <select bind:value={Platform}>
                <option value="Windows 10">Windows 10</option>
                <option value="Windows 11">Windows 11</option>
                <option value="macOS">macOS</option>
                <option value="iOS">iOS</option>
                <option value="Android">Android</option>
                <option value="Linux">Linux</option>
                <option value="Annet">Annet</option>
            </select>
        </td>
        <td>
            <textarea class="textarea" name="Problem" bind:value={Problem} rows="6"></textarea>
        </td> 
        <td>
            <!--Knapp for å sende inn verdiane lagt inn i felta-->
            <button on:click={leggTil}>Legg til</button>
        </td>
    </tr>
</table>
<div class="contentbox">
    <div class="box3">
        <!--Telle oppdrag-->
        <p style="padding-left:15px">Mengde oppdrag: {oppdrag.length}</p>
        <div class="chart-container">
        <!--Lage til søyle for kvar platform, med lengde basert på mengden av platformen i arrayen-->
        {#each Object.entries(count) as [Platform, count]}
        <div class="bar" style="height: {count / oppdrag.length * 90}%;">
        <span class="label">{Platform} ({count})</span>
        </div>
        {/each}
        </div>
    </div>
    <div class="box4">
        <!--Sjekker om det skal vises ei feilmelding, og viser/gøymer den-->
        {#if FeilmeldingStatus == true} 
            <div class="feilmeldingboks">
                <p class="feilmelding">{Feilmelding}</p>
            </div>
        {/if}
    </div>
</div>

