# bandajs2

html 

<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Busca de Bandas em JS!</title>
    <script src="script.js"></script>
    <link rel="stylesheet" href="style.css">
</head>

<body>

  // JAVA SCRIPT 

async function searchBand() {
    const input = document.getElementById('input').value;

    if (input.trim() === '') {
        alert('Por favor, insira o nome de uma banda');
        return;
    }

    try {
        const response = await fetch(`https://musicbrainz.org/ws/2/artist?fmt=json&query=${input}`);
        if (response.ok) {
            const data = await response.json();

            const resultadoDiv = document.getElementById('resultado');
            resultadoDiv.innerHTML = '';

            if (data.artists.length === 0) {
                resultadoDiv.innerHTML = 'Nenhuma banda encontrada';
            } else {
                const artist = data.artists[0];

                const bandInfo = document.createElement('div');
                bandInfo.id = 'banda'
                bandInfo.innerHTML = `
                    <h2>${artist.name}</h2>
                    <p>País de origem: ${artist.country ? artist.country : 'Não disponível'}</p>
                    <p>Ano de formação: ${artist['life-span'] && artist['life-span'].begin ? artist['life-span'].begin : 'Não disponível'}</p>
                    <p>Número de membros: ${artist.members ? artist.members.length : 'Não disponível'}</p>
                    <hr>
                `;
                resultadoDiv.appendChild(bandInfo);

                // Exemplo: criar um link para a página da banda no MusicBrainz
                const bandLink = document.createElement('a');
                bandLink.href = `https://musicbrainz.org/artist/${artist.id}`;
                bandLink.target = '_blank';
                bandLink.textContent = 'Mais informações';
                resultadoDiv.appendChild(bandLink);
            }
        } else {
            alert('Erro ao buscar a banda. Tente novamente mais tarde.');
        }
    } catch (error) {
        console.error('Ocorreu um erro ao buscar a banda:', error);
        alert('Ocorreu um erro ao buscar a banda. Tente novamente mais tarde.');
    }
}

// Event listener para buscar a banda quando a tecla Enter for pressionada no campo de busca
document.getElementById("input").addEventListener("keypress", function(event) {
    if (event.key === "Enter") {
        event.preventDefault();
        searchBand(); 
    }
});
