
ÉTS - LOG430 - Architecture logicielle - Hiver 2026

Étudiant(e) : Laurent St-Hilaire

# Questions
(Il est obligatoire d'ajouter du code, des captures d'écran ou des sorties de terminal pour illustrer chacune de vos réponses.)

## 1.    Quelle est la sortie du terminal que vous obtenez? Si vous répétez cette commande sur yugabyte2 et yugabyte3, est-ce que la sortie est identique? Illustrez votre réponse avec des captures d'écran ou des sorties du terminal.

La réponse est identique sur yugabyte2 et yugabyte3

sh-4.4# ysqlsh -h yugabyte1 -U yugabyte -c "SELECT * FROM orders;"
 id  | user_id | total_amount | payment_link | is_paid |          created_at           
-----+---------+--------------+--------------+---------+-------------------------------
   1 |       1 |         5.75 |              | f       | 2026-03-30 19:16:48.369529+00
 101 |       1 |         5.75 |              | f       | 2026-03-30 19:16:48.414231+00
   2 |       1 |         5.75 |              | f       | 2026-03-30 19:16:48.769293+00
   3 |       2 |         5.75 |              | f       | 2026-03-30 19:16:48.835794+00
(4 rows)


## 2.   Observez la latence moyenne des deux approches affichée dans la sortie du test. Laquelle a la latence moyenne la plus élevée et pourquoi? Illustrez votre réponse avec les sorties du terminal.

La latence pour la méthode optimiste 

 python tests/concurrency_test.py --threads 20 --product 3

============================================================
2026-03-30 19:24:27 - concurrency_test - DEBUG -   Stratégie : Pessimiste (SELECT FOR UPDATE)
2026-03-30 19:24:27 - concurrency_test - DEBUG -   Endpoint  : /orders/pessimistic
2026-03-30 19:24:27 - concurrency_test - DEBUG -   Threads   : 20  (tous libérés simultanément)
2026-03-30 19:24:27 - concurrency_test - DEBUG -   Article   : 3
2026-03-30 19:24:27 - concurrency_test - DEBUG - ============================================================
2026-03-30 19:24:27 - concurrency_test - DEBUG -   Stocks réinitialisés ✓

2026-03-30 19:24:28 - concurrency_test - DEBUG - 
  Résultat : 2 commande(s) réussie(s), 18 échouée(s) sur 20 threads
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Latence moyenne (succès)  : 0.154s
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Latence moyenne (échecs)  : 0.305s
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Latence moyenne (total)   : 0.29s
2026-03-30 19:24:28 - concurrency_test - DEBUG - 

============================================================
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Stratégie : Optimiste  (version + UPDATE conditionnel)
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Endpoint  : /orders/optimistic
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Threads   : 20  (tous libérés simultanément)
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Article   : 3
2026-03-30 19:24:28 - concurrency_test - DEBUG - ============================================================
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Stocks réinitialisés ✓

2026-03-30 19:24:28 - concurrency_test - DEBUG - 
  Résultat : 2 commande(s) réussie(s), 18 échouée(s) sur 20 threads
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Latence moyenne (succès)  : 0.128s
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Latence moyenne (échecs)  : 0.416s
2026-03-30 19:24:28 - concurrency_test - DEBUG -   Latence moyenne (total)   : 0.388s
2026-03-30 19:24:28 - concurrency_test - DEBUG - 
✔ Tests terminés.

# 