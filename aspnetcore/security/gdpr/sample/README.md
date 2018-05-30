# <a name="gdpr-sample"></a><span data-ttu-id="9290a-101">Exemple de PIBR</span><span class="sxs-lookup"><span data-stu-id="9290a-101">GDPR Sample</span></span>

* <span data-ttu-id="9290a-102">Dans *appsettings.json*, définissez `CheckNotConsentNeeded` à `false` pour demander le consentement ; sinon la valeur true ou omettre.</span><span class="sxs-lookup"><span data-stu-id="9290a-102">In *appsettings.json*, set `CheckNotConsentNeeded` to `false` to require consent; otherwise set to true or omit.</span></span> <span data-ttu-id="9290a-103">Tester l’application avec `CheckNotConsentNeeded` la valeur `false` et la valeur `true`.</span><span class="sxs-lookup"><span data-stu-id="9290a-103">Test the app with `CheckNotConsentNeeded` set to `false` and set to `true`.</span></span>
* <span data-ttu-id="9290a-104">Créer des cookies essentielles et les utilisations non vitales avec chaque variation de `CheckConsentNeeded` et l’autorisation accordée.</span><span class="sxs-lookup"><span data-stu-id="9290a-104">Create essential and non-essential cookies with each variation of `CheckConsentNeeded` and consent granted.</span></span>
* <span data-ttu-id="9290a-105">Inscrire un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9290a-105">Register a user.</span></span>
* <span data-ttu-id="9290a-106">Supprimer les cookies.</span><span class="sxs-lookup"><span data-stu-id="9290a-106">Delete cookies.</span></span>