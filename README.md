A C++ ORM, just like MyBatis in Java.

    class Person: public Resource {
    public:


    private:
        int _id;
        std::string _name;
    };


    #define IN
    #define OUT


    class PersonMapper: public Mapper {
    public:
        ///@SQL insert person (id, name) values (#{_id}, #{_name});
        // insert person (id, name) values (?, ?);
        void store(IN Person& person);
        // void store(int id, std::string& name);

        ///@SQL select id, name from person where id=#{id};
        void getOne(IN int id, OUT Person& person);

        ///@SQL select id, name from person;
        void getAll(OUT VectorResource<Person*>& persons);

        ///@SQL select id, name from person;
        void getAll(OUT VectorResource<Person>& persons);
    };


    void example()
    {
        PersonMapper personMapper;

        Person one;
        personMapper.getOne(one);
        cout << one.getId() << ", " << one.getName() << endl;

        one.setName("hello");
        personMapper.store(one);

        VectorResource<Person*> persons;
        personMapper.getAll(persons);
        Person* p1 = persons.get(0);
        cout << p1->getId() << ", "" << p1->getName() << endl;

        VectorResource<Person> personObjs;
        personMapper.getAll(personObjs);
        Person& pObj = personObjs.get(0);
        cout << pObj.getId() << ", " << pObj.getName() << endl;
    }

