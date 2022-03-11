# Créer une command 

- Permet de modifier le comportement d'une méthode

- Créer la méthode `__Construct` pour injecter les dépendances necessaire et déclarer les propriétés :

Exemple :

```sh
class QuestionsDeactivateCommand extends Command
{
    protected static $defaultName = 'app:questions:deactivate';
    protected static $defaultDescription = 'Add a short description for your command';

    private $questionRepo;
    private $entityM;

    public function __construct(QuestionRepository $questionRepository, ManagerRegistry $doctrine)
    {
        parent::__construct();
        $this->questionRepo = $questionRepository;
        $this->entityM = $doctrine->getManager();
    }

    protected function configure(): void
    {
        // $this
        //     ->addArgument('arg1', InputArgument::OPTIONAL, 'Argument description')
        //     ->addOption('option1', null, InputOption::VALUE_NONE, 'Option description')
        // ;
    }

    protected function execute(InputInterface $input, OutputInterface $output): int
    {
        $io = new SymfonyStyle($input, $output);
        // $arg1 = $input->getArgument('arg1');

        // if ($arg1) {
        //     $io->note(sprintf('You passed an argument: %s', $arg1));
        // }

        // if ($input->getOption('option1')) {
        //     // ...
        // }

        // récupérer toutes les questions ( qui sont active )
        $allQuestions = $this->questionRepo->findBy(['active' => true]);

        // pour chaque question 
        foreach ($allQuestions as $currentQuestion)
        {
            if ($this->questionNeedDeactivation($currentQuestion))
            {
                $currentQuestion->setActive(false);
            }
        }

        // exécuter les requêtes
        $this->entityM->flush();


        $io->success('Deactivation done !');

        return Command::SUCCESS;
    }
    ```