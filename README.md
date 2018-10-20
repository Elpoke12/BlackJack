# BlackJack

class BlackJack
	{
        static Player[] players = new Player[5];
		static int puntero = 0;

		class CartaEnJuego
		{
			public string Simbolo;
			public int Valor;
			public int Points;

			// Constructor alternativo con 2 parámetros. - Int for Suit, Int for Value
			// Usamos esto en el generarDeck() 
			public CartaEnJuego(int s, int v)
			{
                Valor = v; 
				switch (s) 
				{
					case 1: 
                        Simbolo = "♣";
						break;
					case 2: 
                        Simbolo = "♦";
						break;
					case 3: 
                        Simbolo = "♥";
						break;
					case 4: 
                        Simbolo = "♠";
						break;
				}

                if (Valor > 10)
				{
					Points = 10;
				}
                else if (Valor == 1) // Y si no es un as
				{
					Points = 11; // Pon los puntos a 11.
				}
				else
				{
                    Points = Valor;
				}
			}
		}

		class Player
		{
			public CartaEnJuego[] mano;
			public int CartasEnLaMano;
			public int points;
			public string Nombre;

			public Player()
			{
                mano = new CartaEnJuego[5];
				CartasEnLaMano = 0;
				points = 0;
			}

		}

		static void Main(string[] args)
		{
			onePlayer();

			Console.ReadLine();

		}

		// Genera la baraja de 52 cartas
		static CartaEnJuego[] generarDeck()
		{
            CartaEnJuego[] deck = new CartaEnJuego[52]; 
			int contador = 0; 

			
            for (int simbolo = 1; simbolo < 5; simbolo++) 
			{
				for (int value = 1; value < 14; value++) 
				{
                    deck[contador] = new CartaEnJuego(simbolo, value); // Genera una nueva carta y la guarda en la baraja.
					contador++; 
				}
			}

            return deck; // Regresa la baraja completa
		}

		// Barajea las cartas
		static void shuffleDeck(ref CartaEnJuego[] deck)
		{
			Random rnd = new Random(); 
			CartaEnJuego temp; 
			int num; 

			for (int i = 0; i < deck.Length; i++) 
			{
				num = rnd.Next(0, deck.Length);
				
				temp = deck[i];
				deck[i] = deck[num];
				deck[num] = temp;
			}

			
		}

		static void SalidaDeCarta(CartaEnJuego card)
		{
            switch (card.Valor) // Aqui se le da valor a las cartas
			{
				
				case 1:
                    Console.WriteLine("Ace de {0}", card.Simbolo);
					break;

				
				case 11:
                    Console.WriteLine("Jack de {0}", card.Simbolo);
					break;

				
				case 12:
                    Console.WriteLine("Reina de {0}", card.Simbolo);
					break;

				
				case 13:
                    Console.WriteLine("Rey de {0}", card.Simbolo);
					break;
				 
				default:
                    Console.WriteLine("The {0} of {1}", card.Valor, card.Simbolo);
					break;
			}
		}

		// Despliega los detalles de las cartas usando símbolos 
		// Se utiliza para mostrar la mano del jugador en la misma línea 
		static void DespligueSimDeCarta(CartaEnJuego carta)
		{
            switch (carta.Valor) 
			{
				
				case 1:
                    Console.Write("As{0} ", carta.Simbolo);
					break;

				
				case 11:
                    Console.Write("Jack{0} ", carta.Simbolo);
					break;

				
				case 12:
                    Console.Write("Reina{0} ", carta.Simbolo);
					break;

				
				case 13:
                    Console.Write("Rey{0} ", carta.Simbolo);
					break;
				
				default:
                    Console.Write("{0}{1} ", carta.Valor, carta.Simbolo);
					break;
			}
		}

		// Muestra todas las cartas en la mano del jugador junto con su total de puntos
		static void SalidaMano(Player player)
		{
			// Despliga Mano Actual
			Console.Write("Mano Actual: ");
			// Recorre todas las cartas en la mano.
			for (int i = 0; i < player.CartasEnLaMano; i++)
			{
                DespligueSimDeCarta(player.mano[i]);
			}
			Console.WriteLine("Tus puntos: {0}.", player.points);
		}

		static void JalaCarta(CartaEnJuego[] deck, ref Player player)
		{
			CartaEnJuego SiguienteCarta = deck[puntero];

			// Añade la siguiente carta en el mazo a la mano del jugador.
			if (player.CartasEnLaMano < 5)
			{
                player.mano[player.CartasEnLaMano] = SiguienteCarta;

				// Incrementa el número de cartas en la mano del jugador.
				player.CartasEnLaMano++;

				// Agrega el valor en puntos de la nueva carta al total del jugador
				player.points += SiguienteCarta.Points;

			
				puntero++;
			}
		}

		// Comprueba si el jugador ha superado los 21 puntos.
		static bool checkPoints(Player player)
		{

			// Comprueba si el jugador aun tiene puntos validos
			if (player.points > 21)
			{
				Console.WriteLine("Perdiste Crack");
				return false;
			}

			// Devuelve true si el jugador sigue en juego
			return true;
		}

		// Compare Compara los jugadores
		static void Gandador(Player player, Player Opponent)
		{
			// El jugador gana si... 
            if (Opponent.points > 21 || player.CartasEnLaMano == 5 && Opponent.CartasEnLaMano != 5)
			{
                Console.WriteLine("{0} Ganaste, maquina", player.Nombre);
			}

			// El juego termina en un empate si... 
			else if (Opponent.points == player.points)
			{
				Console.WriteLine("Empate");
			}
			// De lo contrario, el oponente gana
			else if (Opponent.points == 21 && player.points != 21 || player.CartasEnLaMano < 5)
			{
                Console.WriteLine("{0} Ganaste", Opponent.points);
			}
            else if (player.CartasEnLaMano == 5 && Opponent.CartasEnLaMano == 5)
			{
                if (player.points > Opponent.points)
				{
                    Console.WriteLine("{0} Ganaste! 5 juego de cartas: más alto que el oponente. ", player.Nombre);
				}

                else if (player.points == Opponent.points)
				{
					Console.WriteLine("Es un empate Crack! 5 juego de cartas: iguales ");
				}

                Console.WriteLine("{0} Ganaste! 5 juego de cartas: menos que el oponente. ", Opponent.Nombre);
			}
		}

		// Comprueba si el jugador tiene algún ases con un valor de puntos de 11
		// Si el jugador está a punto de perder, cambia el as a un valor de 1 punto 
		// Luego actualiza la puntuación del jugador
		static void checkAces(ref Player player)
		{
			bool cambio = false; // Señala si ya hemos cambiado un as
			if (player.points > 21)
			{
				for (int i = 0; i < player.CartasEnLaMano; i++)
				{
                    if (player.mano[i].Points == 11 && cambio == false) // Si es un as alto
					{
                        player.mano[i].Points = 1; // Cambia a un as bajo
						player.points -= 10; // Quita 10 puntos del jugador
						cambio = true;
					}
				}
			}

		}

		static void onePlayer()
		{
			string JugarDeNuevo = "Indefinido";
			do
			{
				// Genera la baraja y lo barajea
                CartaEnJuego[] deck = generarDeck();
				shuffleDeck(ref deck);

				// Se crean los 2 jugadores
				Player player = new Player();
				Console.Write("Introduzca su nombre ");
                player.Nombre = Console.ReadLine();

				Player Oponente = new Player();
				Console.Write("Introduzca el nombre de su oponente: ");
                Oponente.Nombre = Console.ReadLine();

				// Jala las 2 primeras cartas para el jugador
                JalaCarta(deck, ref player);
                JalaCarta(deck, ref player);


				checkAces(ref player); 
                SalidaMano(player);
				checkPoints(player); // Despliega los puntos del jugador
				bool Enjuego = true;

				string Decision = "Indefinido";

                while (Enjuego == true && Decision != "Jalar")
				{
					Console.Write("Desea Tirar o Jalar? ");
                    Decision = Console.ReadLine();
                    if (Decision == "Tirar") // Si desea tirar entonces...
					{
                        JalaCarta(deck, ref player);

						// Si el jugador tiene puntos validos aun, EnJuego sera verdad 
						// Si el jugador no tiene puntos validos Enjuego sera Falso y se saldra del ciclo
						checkAces(ref player); 
                        SalidaMano(player);
                        Enjuego = checkPoints(player);
					}
				}
				// Si el jugador no ha perdido, es el turno del oponente
				if (Enjuego== true)
				{
					bool OponenteEnJuego = true;

					Console.WriteLine();
					Console.WriteLine("*** Turno del Oponente ***");
                    JalaCarta(deck, ref Oponente);
                    JalaCarta(deck, ref Oponente);

                    checkAces(ref Oponente); 
                    SalidaMano(Oponente);
                    checkPoints(Oponente);

                    while (OponenteEnJuego == true)
					{
						Console.WriteLine("Precione Enter para continuar");
						Console.ReadLine();

						// Jala la sigueinte carta del opnente y verifica si aun tiene puntos validos 
                        JalaCarta(deck, ref Oponente);

                        checkAces(ref Oponente); 
                        SalidaMano(Oponente);
                        OponenteEnJuego = checkPoints(Oponente);
					}
				}

				// Calcula el ganador
                Gandador(player, Oponente);

				Console.Write("Quiere jugar de nuevo? Si/No ");
				JugarDeNuevo = Console.ReadLine();
			} while (JugarDeNuevo == "Si");
		}
   }
   
