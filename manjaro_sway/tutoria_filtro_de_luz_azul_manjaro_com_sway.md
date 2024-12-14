# Blue Light Filter
O filtro de luz azul é algo muito almejaado por muito programadores e pessoas
que possuem algum problema relacionado a sensibilizade deste canal. Além das
pessoas que utilizam essa coloração por causar um conforto nos olhos.

## Revisões
| Revisor | Data de Revisão |
| :----: | ----: |
| [Arthur Gorgônio](https://github.com/Arthurgorgonio) | 14/12/2024 |
| [Amaro Júnior](https://github.com/porfirioamarojr) | 08/04/2023 |

---

## Configurando

Primeiro instale o gammastep com
```sh
sudo pacman -S gammastep
```

Para ver se o `gammastep` está funcionando corretamente utilize com a opção `-V`
para testar isso. Agora que ele está instalado, pode ser utilizada a opção `-h`
para apresentar o help.

Mas antes que você prossiga, é necessário que você saiba a sua geolocalização.
Essa configuração pode ser feita manualmente por meio da api
[iplocation](https://www.iplocation.net/myip) ou utilizando a localização
automática do _geoclue_.

## Adicionando às configurações do SwayWM (i3WM)
De posse disso, só falta iniciar o `gammastep` juntamente com o sistema
operacional ou a sua interface gráfica. Neste tutorial será demonstrado essa
alteração no [swaywm](https://swaywm.org/), alterando o arquivo de configuração
do usuário, `/.config/sway/config`, agora dentro desse arquivo adicione a
seguinte linha:
```
exec gammastep -l -6.:-37 -t 6500:4500 -g 1.0 -b 1.0:0.6 -m wayland
```
Após isso, salve o arquivo e recarregue a interface. Agora o seu sistema irá
iniciar o serviço automaticamente.

## Configuração por meio de atalhos
Nem sempre queremos que a cor altere por estarmos apresentando a tela ou fazendo
alguma tarefa específica. Outras vezes queremos que o tom na cor quente fique
mais brando. Para contornar isso, vamos configurar um atalho no SwayWM para que
seja possível alterar o valor do `gammastep` sem a necessidade de ficar abrindo
o arquivo de configuração todas as vezes e atualizar o valor.

Neste sentido, vamos assumir as seguintes faixas:
- 6500 Diurno (_day_)
- 5500 Transição (_trainsition_)
- 4500 Noturno (_night_)

De posse do seu arquivo de configuração, adicione as seguintes linhas no arquivo:
```
set $mode_gamma GammaStep: (d)ay, (t)ransition, (n)ight, (i)nit ou (k)ill
set $gamma gammastep -l -6.4:-37.1 -g 1.0 -b 1.0:1.0 -l manual -m wayland
set $kill_gamma killall gammastep
bindsym $mod+t mode '$mode_gamma'  # pode alterar para o atalho de preferência
mode '$mode_gamma' {
  bindsym d exec $kill_gamma && $gamma -O 6500, mode 'default'
  bindsym t exec $kill_gamma && $gamma -O 5500, mode 'default'
  bindsym n exec $kill_gamma && $gamma -O 4500, mode 'default'
  bindsym i exec $gamma -O 6500, mode 'default'  # opcional, observe a nota1
  bindsym k exec $kill_gamma, mode 'default'  # opcional, se for usar, observe a nota2
  bindsym Escape mode 'default'
}
```
**Notas**:
1. _Garante uma forma de iniciar o `gammastep`, caso opte por não inicializá-lo junto
do sistema. Assim, evita ficar travado, caso mate o processo do `gammastep` e depois
deseje iniciá-lo com outra temperatura._
2. _É possível fazer uso do && ou ; para separar os comandos. Neste tutorial eu vou
optar pelo `&&`, pois ele garante que caso o comando `killall gamastep` falhar
(retorno diferente de 0), o _gammastep_ não será inicializado._
3. _Caso utilize o `&&`. Se utilizar a opção `k`, deve-se, obrigatoriamente, utilizar
a opção `i` antes de altenar entre outras temperaturas, pois é necessário uma instância
do programa para poder executar com sucesso._

Agora, salve e recarregue o arquivo da interface, com isso será possível
alternar entre as temperaturas de modo simplificado.


## Referências
- https://man.archlinux.org/man/extra/gammastep/gammastep.1.en
- https://www.mankier.com/5/geoclue#Synopsis
- https://github.com/Madic-/Sway-DE/issues/7
