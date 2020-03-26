void Unos(const char *, Kurs **nizKurseva, int *brojKurseva, Prijava **nizPrijava, int *brojPrijava)

bool DodajKurs(Kurs *dodaj)//argument je kurs koji pokusavamo dodati
{
    if(this->_trenutnoKurseva == 50)//ne smije biti preko 50 kurseva
        return 0;
    for(int i = 0; i < this->trenutnoKurseva; i++) //petlja za provjeru da li dati kurs vec postoji
        if(strcmp(dodaj->_kurs, this->_kursevi[i]._kurs) == 0)
            return 0;\
    this->niz[this->_trenutnoKurseva++] = dodaj;
    return 1;
}

bool DodajPrijavu(Kurs *kurs, Polaznik *pol)//Prijava polaznika (pol) na kurs (kurs)
{
    bool exitParam = 1;//pomocna varijabla koju koristimo kao izlazni parametar u slucaju da dodavanje prijave nije moguce
    for(int i = 0; i < this->_trenutnoKurseva && exitParam != 0; i++) //petlja u kojoj provjeravamo da li dati kurs postoji u centru
        if(strcmp(kurs->_kurs, this->_kursevi[i]._kurs) == 0)
            exitParam = 0;
    if(exitParam || *(pol->_datumRodjenja->godina) > 2002)// provjera da li je osoba punoljetna
        return 0;
    else
    {
        this->_prijave[this->_trenutnoPrijava++] = *pol;//ako je polaznik punoljetan i ako je dati kurs registrovan dodajemo prijavu u niz
        return 1;//vracamo 1 jer je funkcija obavila dodavanje prijave
    }
}

void Ispis(SkillsCentar *centar)
{
    std::cout << "Polaznici koji pohadjaju kurs 'Programiranje': " << std::endl;
    for(int i = 0; i < centar->_trenutnoPrijava; i++)//petlja koja prolazi kroz sve polaznike i provjerava da li idu na kurs 'Programiranje'
        if(strcmp(centar->_prijave[i]._kurs._kurs, "Programiranje") == 0)
            std::cout << centar->_prijave[i]._imePrezime << std::endl;
}

int Prebroji(SkillsCentar *centar, const char *str)
{
    int broj = 0;
    std::cout << "Kursevi koji u svom nazivu sadrze rijec '" << str <<"':"  << std::endl;
    for(int i = 0; i < centar->_trenutnoKurseva; i++)//petlja koja prolazi kroz sve kurseve i provjerava da li u svom nazivu sadrze rijec 'informatika'
        if(strstr(centar->_kursevi[i]._kurs, str) != NULL)//funkcija koja provjerava da li se string 'informatika' nalazi u nazivu kursa 
        {
            std::cout << centar->_prijave[i]._imePrezime << std::endl;
            broj++;
        }
}

void main()
{
    SkillsCentar cent;
    Polaznik p1, p2, p3;
    cent.DodajKurs("Programiranje");
    cent.DodajKurs("Matematika");
    cent.DodajKurs("Informatika");
    cent.DodajKurs("Poslovna informatika");
    cent.DodajPrijavu("Graficki dizajn", &p1) //biti ce neuspjesno jer taj kurs jos uvijek ne postoji
    cent.DodajKurs("Graficki dizajn");
    cent.DodajPrijavu("Graficki dizajn", &p1) //sad ce biti uspjesno
    cent.DodajPrijavu("Programiranje", &p2)
    Ispis(&cent);//nece ispisati imena jer nismo imenovali polaznike
    int br = Prebroji(cent, 'informatika');
    std::cout << "Ukupan broj kurseva koji sadrzi rijec 'informatika' je : " << br << std::endl;
    system("pause");
}
