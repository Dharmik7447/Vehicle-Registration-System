# Vehicle-Registration-System
cout << 
#include <iostream>
#include <string>

using namespace std;

class Vehicle
{
public:
    int vehicleID;
    string manufacturer;
    string model;
    int year;

    static int totalVehicles;

    Vehicle()
    {
        vehicleID = 0;
        manufacturer = "";
        model = "";
        year = 0;

        totalVehicles++;
    }

    Vehicle(int ID, const string &manufacture, const string &mod, int y)
    {
        vehicleID = ID;
        manufacturer = manufacture;
        model = mod;
        year = y;

        totalVehicles++;
    }

    ~Vehicle()
    {
        totalVehicles--;
    }

    void getdata()
    {
        cout << "Enter your vehicle ID: " << endl;
        cin >> vehicleID;

        cout << "Manufactured by: " << endl;
        cin >> manufacturer;

        cout << "Enter your vehicle Model: " << endl;
        cin >> model;

        cout << "Enter your vehicle manufactured year: " << endl;
        cin >> year;
    }
    void setdata()
    {
        cout << "Vehicle ID: " << vehicleID << endl;

        cout << "Manufactured by: " << manufacturer << endl;

        cout << "vehicle Model: " << model << endl;

        cout << "vehicle manufactured year: " << year << endl;
    }
};

class Car : public virtual Vehicle
{
public:
    string fuelType;

    void getdata()
    {
        Vehicle ::getdata();
        cout << "Enter fuel type: " << endl;
        cin >> fuelType;
    }

    void setdata()
    {
        Vehicle::setdata();
        cout << "Fuel Type: " << fuelType << endl;
    }
};

class ElectricCar : public Car
{
public:
    string BatteryCapacity;

    void getdata()
    {
        Car::getdata();
        cout << "Enter Battery capacity: " << endl;
        cin >> BatteryCapacity;
    }

    void setdata()
    {
        Vehicle::setdata();
        cout << "Battery Capacity: " << BatteryCapacity << endl;
    }
};

class Aircraft : public virtual Vehicle
{
public:
    string FlightRange;

    void getdata()
    {
        cout << "Enter your Flight's Range: " << endl;
        cin >> FlightRange;
    }

    void setdata()
    {
        Vehicle::setdata();
        cout << "Flight Range: " << FlightRange << endl;
    }
};

class FlyingCar : public Car, public Aircraft
{
public:
    void getdata()
    {
        Car::getdata();
        Aircraft::getdata();
    }

    void setdata()
    {
        Car::setdata();
        Aircraft::setdata();
    }
};
class SportsCar : public ElectricCar
{
public:
    string TopSpeed;

    void getdata()
    {
        ElectricCar::getdata();
        cout << "Enter Your Sports Car's Top Speed: " << endl;
        cin >> TopSpeed;
    }

    void setdata()
    {
        ElectricCar::setdata();
        cout << "Sports Car's Top Speed: " << TopSpeed << endl;
    }
};


class Sedan : public Car
{
public:
    void getdata()
    {
        Car::getdata();
    }

    void setdata()
    {
        Car::setdata();
    }
};

class SUV : public Car
{
public:
    void getdata()
    {
        Car::getdata();
    }

    void setdata()
    {
        Car::setdata();
    }
};
class VehicleRegistry
{
private:
    Vehicle* vehicles[100];
    int count;
public:
    VehicleRegistry()
    {
        count = 0;
    }
    ~VehicleRegistry()
    {
        for(int i = 0; i < count; i++)
        {
            delete vehicles[i];
        }
    }
    void AddVehicle(Vehicle* v)
    {
        if(count < 100)
            vehicles[count++] = v;
        else
        {
            cout << "Registry full!!!" << endl;
        }
    }

    void DisplayAll()
    {
        for(int i = 0; i < count; i++)
        {
            vehicles[i]->setdata();
            cout << "----------------------" << endl;
        }
    }

    void SearchByID(int ID)
    {
        for(int i = 0; i < count; i++)
        {
            if(vehicles[i]->vehicleID == ID)
            {
                vehicles[i]->setdata();
                return;
            }
        }
        cout << "Vehicle not found!!" << endl;
    }
};

int Vehicle::totalVehicles = 0;

int main()
{
    VehicleRegistry registry;

    int choice;

    do
    {
        cout << endl << "Welcome to Vehicle Registry System!!!" << endl;
        cout << endl << "1. Add a Vehicle" << endl;
        cout << "2. View All Vehicles" << endl;
        cout << "3. Search by ID" << endl;
        cout << "4. Exit" << endl;
        cout << endl << "Enter choice: ";
        cin >> choice;

        switch(choice)
        {
            case 1:
            {
                int type;
                cout << "Select Vehicle Type:" << endl;
                cout << "1. Car" << endl;
                cout << "2. Aircraft" << endl;
                cin >> type;

                if(type == 1)
                {
                    int select;
                    cout << endl << "Select Type of Car:" << endl;
                    cout << "1. Electric Car" << endl;
                    cout << "2. Flying Car" << endl;
                    cout << "3. Sports Car" << endl;
                    cin >> select;

                    if(select == 1)
                    {
                        int enter;
                        cout << endl << "Enter Type of Electric Car: " << endl;
                        cout << "1. Sedan" << endl;
                        cout << "2. SUV" << endl;
                        cin >> enter;

                        if(enter == 1)
                        {
                            Sedan* sedan = new Sedan();
                            sedan->getdata();
                            registry.AddVehicle(sedan);
                        }
                        else if(enter == 2)
                        {
                            SUV* suv = new SUV();
                            suv->getdata();
                            registry.AddVehicle(suv);
                        }
                        else
                        {
                            cout << "Invalid Choice!!" << endl;
                        }
                    }
                    else if(select == 2)
                    {
                        FlyingCar* flyingCar = new FlyingCar();
                        flyingCar->getdata();
                        registry.AddVehicle(flyingCar);
                    }
                    else if(select == 3)
                    {
                        SportsCar* sportsCar = new SportsCar();
                        sportsCar->getdata();
                        registry.AddVehicle(sportsCar);
                    }
                    else
                    {
                        cout << "Invalid Choice!!" << endl;
                    }
                }
                else if(type == 2)
                {
                    Aircraft* aircraft = new Aircraft();
                    aircraft->getdata();
                    registry.AddVehicle(aircraft);
                }
                else
                {
                    cout << "Invalid Choice!!" << endl;
                }
                break;
            }
            case 2:
            {
                registry.DisplayAll();
                break;
            }
            case 3:
            {
                int id;
                cout << "Enter Vehicle ID to search: ";
                cin >> id;
                registry.SearchByID(id);
                break;
            }
            case 4:
            {
                cout << "Exiting..." << endl;
                break;
            }
            default:
                cout << "Invalid Choice!!" << endl;
        }
    }
    while (choice != 4);

    return 0;
}
