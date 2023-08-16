import simpy
import random
import numpy as np
import csv

class Household:
    def __init__(self, env, epc_rating, imd_quintile, monthly_parameters):
        self.env = env
        self.epc_rating = epc_rating
        self.imd_quintile = imd_quintile
        self.monthly_parameters = monthly_parameters
        self.total_consumption = 0
        self.monthly_consumptions = []

    def generate_monthly_energy_usage(self, month):
        current_month = (month - 1) % 12 + 1
        epc_mean_consumption = self.monthly_parameters['EPC'][self.epc_rating][current_month]['mean_consumption'] * 30
        epc_std_consumption = self.monthly_parameters['EPC'][self.epc_rating][current_month]['std_consumption'] * 30
        imd_mean_consumption = self.monthly_parameters['IMD'][self.imd_quintile][current_month]['mean_consumption'] * 30
        imd_std_consumption = self.monthly_parameters['IMD'][self.imd_quintile][current_month]['std_consumption'] * 30

        epc_consumption = max(0, np.random.normal(epc_mean_consumption, epc_std_consumption))
        imd_consumption = max(0, np.random.normal(imd_mean_consumption, imd_std_consumption))
        return (epc_consumption + imd_consumption)/2



    def run(self):
        while True:
            current_month = (int(self.env.now) % 365) // 30 + 1
            monthly_energy_usage = self.generate_monthly_energy_usage(current_month)
            self.total_consumption += monthly_energy_usage
            self.monthly_consumptions.append(monthly_energy_usage)
            yield self.env.timeout(30)

def generate_random_households(env, num_households, epc_ratings, imd_quintiles, monthly_parameters):
    households = []
    num_fuel_poor = 13  # Number of households with high risk attributes
    num_non_FP = num_households - num_fuel_poor

    for _ in range(num_fuel_poor):
        epc_rating = random.choice(['D', 'E', 'F', 'G'])
        imd_quintile = random.choice([1, 2])

        household = Household(env, epc_rating, imd_quintile, monthly_parameters)
        households.append(household)
        env.process(household.run())

    for _ in range(num_non_FP):
        epc_rating = random.choice([rating for rating in epc_ratings if rating not in ['D', 'E', 'F', 'G']])
        imd_quintile = random.choice([3, 4, 5])

        household = Household(env, epc_rating, imd_quintile, monthly_parameters)
        households.append(household)
        env.process(household.run())

    random.shuffle(households)
    return households

if __name__ == "__main__":
    num_simulations = 100
    num_households = 100
    num_months = 12
    epc_ratings = ['A', 'B', 'C', 'D', 'E', 'F', 'G']
    imd_quintiles = [1, 2, 3, 4, 5]


    monthly_parameters = {
        'EPC': {
            'A': {1: {'mean_consumption': 12.321, 'std_consumption': 12.321},
                  2: {'mean_consumption': 11.193, 'std_consumption': 11.166},
                  3: {'mean_consumption': 9.243, 'std_consumption': 9.068},
                  4: {'mean_consumption': 7.202, 'std_consumption': 9.244},
                  5: {'mean_consumption': 6.504, 'std_consumption': 7.37},
                  6: {'mean_consumption': 5.07, 'std_consumption': 6.906},
                  7: {'mean_consumption': 5.088, 'std_consumption': 6.49},
                  8: {'mean_consumption': 5.663, 'std_consumption': 5.91},
                  9: {'mean_consumption': 6.065, 'std_consumption': 5.635},
                  10: {'mean_consumption': 7.741, 'std_consumption': 6.094},
                  11: {'mean_consumption': 9.465, 'std_consumption': 7.816},
                  12: {'mean_consumption': 10.488, 'std_consumption': 8.865}},
            'B': {1: {'mean_consumption': 12.321, 'std_consumption': 12.321},
                  2: {'mean_consumption': 11.193, 'std_consumption': 11.166},
                  3: {'mean_consumption': 9.243, 'std_consumption': 9.068},
                  4: {'mean_consumption': 7.202, 'std_consumption': 9.244},
                  5: {'mean_consumption': 6.504, 'std_consumption': 7.37},
                  6: {'mean_consumption': 5.07, 'std_consumption': 6.906},
                  7: {'mean_consumption': 5.088, 'std_consumption': 6.49},
                  8: {'mean_consumption': 5.663, 'std_consumption': 5.91},
                  9: {'mean_consumption': 6.065, 'std_consumption': 5.635},
                  10: {'mean_consumption': 7.741, 'std_consumption': 6.094},
                  11: {'mean_consumption': 9.465, 'std_consumption': 7.816},
                  12: {'mean_consumption': 10.488, 'std_consumption': 8.865}},
            'C': {
                1: {'mean_consumption': 11.871, 'std_consumption': 11.502},
                2: {'mean_consumption': 11.058, 'std_consumption': 10.409},
                3: {'mean_consumption': 9.577, 'std_consumption': 8.528},
                4: {'mean_consumption': 7.999, 'std_consumption': 7.526},
                5: {'mean_consumption': 7.333, 'std_consumption': 6.354},
                6: {'mean_consumption': 5.973, 'std_consumption': 5.549},
                7: {'mean_consumption': 5.949, 'std_consumption': 5.337},
                8: {'mean_consumption': 6.172, 'std_consumption': 4.949},
                9: {'mean_consumption': 6.545, 'std_consumption': 4.92},
                10: {'mean_consumption': 7.645, 'std_consumption': 5.45},
                11: {'mean_consumption': 8.951, 'std_consumption': 7.095},
                12: {'mean_consumption': 9.871, 'std_consumption': 7.707},
            },
            'D': {
                1: {'mean_consumption': 13.641, 'std_consumption': 13.144},
                2: {'mean_consumption': 12.783, 'std_consumption': 12.22},
                3: {'mean_consumption': 11.44, 'std_consumption': 18.479},
                4: {'mean_consumption': 9.956, 'std_consumption': 20.035},
                5: {'mean_consumption': 9.09, 'std_consumption': 21.134},
                6: {'mean_consumption': 7.64, 'std_consumption': 25.472},
                7: {'mean_consumption': 7.367, 'std_consumption': 19.191},
                8: {'mean_consumption': 7.829, 'std_consumption': 25.473},
                9: {'mean_consumption': 7.578, 'std_consumption': 5.838},
                10: {'mean_consumption': 8.845, 'std_consumption': 7.183},
                11: {'mean_consumption': 10.326, 'std_consumption': 9.366},
                12: {'mean_consumption': 11.307, 'std_consumption': 10.559},
            },
            'E': {
                1: {'mean_consumption': 15.422, 'std_consumption': 13.078},
                2: {'mean_consumption': 14.626, 'std_consumption': 12.08},
                3: {'mean_consumption': 12.855, 'std_consumption': 10.133},
                4: {'mean_consumption': 11.56, 'std_consumption': 9.38},
                5: {'mean_consumption': 10.346, 'std_consumption': 7.666},
                6: {'mean_consumption': 8.46, 'std_consumption': 6.458},
                7: {'mean_consumption': 8.528, 'std_consumption': 6.581},
                8: {'mean_consumption': 8.42, 'std_consumption': 6.405},
                9: {'mean_consumption': 8.731, 'std_consumption': 6.063},
                10: {'mean_consumption': 10.023, 'std_consumption': 7.013},
                11: {'mean_consumption': 12.007, 'std_consumption': 9.38},
                12: {'mean_consumption': 13.274, 'std_consumption': 10.72},
            },
            'F': {
                1: {'mean_consumption': 19.558, 'std_consumption': 22.434},
                2: {'mean_consumption': 18.418, 'std_consumption': 21.039},
                3: {'mean_consumption': 15.505, 'std_consumption': 17.101},
                4: {'mean_consumption': 13.532, 'std_consumption': 15.43},
                5: {'mean_consumption': 11.793, 'std_consumption': 11.833},
                6: {'mean_consumption': 8.955, 'std_consumption': 7.599},
                7: {'mean_consumption': 8.866, 'std_consumption': 7.088},
                8: {'mean_consumption': 8.922, 'std_consumption': 7.53},
                9: {'mean_consumption': 9.227, 'std_consumption': 6.953},
                10: {'mean_consumption': 11.192, 'std_consumption': 9.515},
                11: {'mean_consumption': 13.016, 'std_consumption': 10.947},
                12: {'mean_consumption': 14.748, 'std_consumption': 13.59},
            },
            'G': {
                1: {'mean_consumption': 19.558, 'std_consumption': 22.434},
                2: {'mean_consumption': 18.418, 'std_consumption': 21.039},
                3: {'mean_consumption': 15.505, 'std_consumption': 17.101},
                4: {'mean_consumption': 13.532, 'std_consumption': 15.43},
                5: {'mean_consumption': 11.793, 'std_consumption': 11.833},
                6: {'mean_consumption': 8.955, 'std_consumption': 7.599},
                7: {'mean_consumption': 8.866, 'std_consumption': 7.088},
                8: {'mean_consumption': 8.922, 'std_consumption': 7.53},
                9: {'mean_consumption': 9.227, 'std_consumption': 6.953},
                10: {'mean_consumption': 11.192, 'std_consumption': 9.515},
                11: {'mean_consumption': 13.016, 'std_consumption': 10.947},
                12: {'mean_consumption': 14.748, 'std_consumption': 13.59},
            },
        },
        'IMD': {
            1: {1: {'mean_consumption': 11.474, 'std_consumption': 11.666},
                2: {'mean_consumption': 10.845, 'std_consumption': 9.967},
                3: {'mean_consumption': 9.879, 'std_consumption': 18.596},
                4: {'mean_consumption': 8.879, 'std_consumption': 20.604},
                5: {'mean_consumption': 8.121, 'std_consumption': 22.041},
                6: {'mean_consumption': 6.748, 'std_consumption': 5.708},
                7: {'mean_consumption': 6.353, 'std_consumption': 4.494},
                8: {'mean_consumption': 6.406, 'std_consumption': 4.278},
                9: {'mean_consumption': 6.624, 'std_consumption': 4.406},
                10: {'mean_consumption': 7.392, 'std_consumption': 4.785},
                11: {'mean_consumption': 8.372, 'std_consumption': 5.871},
                12: {'mean_consumption': 9.131, 'std_consumption': 6.805}},
            2: {1: {'mean_consumption': 12.943, 'std_consumption': 11.219},
                2: {'mean_consumption': 12.208, 'std_consumption': 10.538},
                3: {'mean_consumption': 10.723, 'std_consumption': 9.138},
                4: {'mean_consumption': 9.38, 'std_consumption': 8.483},
                5: {'mean_consumption': 8.385, 'std_consumption': 6.792},
                6: {'mean_consumption': 6.779, 'std_consumption': 5.329},
                7: {'mean_consumption': 6.769, 'std_consumption': 5.226},
                8: {'mean_consumption': 6.825, 'std_consumption': 4.92},
                9: {'mean_consumption': 7.099, 'std_consumption': 4.924},
                10: {'mean_consumption': 8.225, 'std_consumption': 5.895},
                11: {'mean_consumption': 9.651, 'std_consumption': 7.631},
                12: {'mean_consumption': 10.664, 'std_consumption': 8.839}},
            3: {1: {'mean_consumption': 15.116, 'std_consumption': 14.674},
                2: {'mean_consumption': 14.213, 'std_consumption': 13.394},
                3: {'mean_consumption': 12.333, 'std_consumption': 11.262},
                4: {'mean_consumption': 10.776, 'std_consumption': 10.381},
                5: {'mean_consumption': 9.557, 'std_consumption': 8.325},
                6: {'mean_consumption': 7.896, 'std_consumption': 16.53},
                7: {'mean_consumption': 7.964, 'std_consumption': 21.392},
                8: {'mean_consumption': 8.536, 'std_consumption': 28.548},
                9: {'mean_consumption': 8.03, 'std_consumption': 6.021},
                10: {'mean_consumption': 9.405, 'std_consumption': 7.093},
                11: {'mean_consumption': 11.098, 'std_consumption': 9.124},
                12: {'mean_consumption': 12.393, 'std_consumption': 10.587}},
            4: {1: {'mean_consumption': 14.733, 'std_consumption': 13.811},
                2: {'mean_consumption': 13.697, 'std_consumption': 13.004},
                3: {'mean_consumption': 12.109, 'std_consumption': 16.517},
                4: {'mean_consumption': 10.64, 'std_consumption': 20.124},
                5: {'mean_consumption': 9.748, 'std_consumption': 21.919},
                6: {'mean_consumption': 8.021, 'std_consumption': 23.066},
                7: {'mean_consumption': 8.076, 'std_consumption': 24.563},
                8: {'mean_consumption': 7.843, 'std_consumption': 6.593},
                9: {'mean_consumption': 8.132, 'std_consumption': 6.251},
                10: {'mean_consumption': 9.578, 'std_consumption': 7.942},
                11: {'mean_consumption': 11.167, 'std_consumption': 10.29},
                12: {'mean_consumption': 12.449, 'std_consumption': 11.833}},
            5: {1: {'mean_consumption': 15.51, 'std_consumption': 13.471},
                2: {'mean_consumption': 14.382, 'std_consumption': 12.121},
                3: {'mean_consumption': 12.733, 'std_consumption': 10.254},
                4: {'mean_consumption': 11.189, 'std_consumption': 9.85},
                5: {'mean_consumption': 10.085, 'std_consumption': 8.064},
                6: {'mean_consumption': 8.372, 'std_consumption': 6.779},
                7: {'mean_consumption': 8.359, 'std_consumption': 6.656},
                8: {'mean_consumption': 8.535, 'std_consumption': 6.178},
                9: {'mean_consumption': 8.815, 'std_consumption': 6.083},
                10: {'mean_consumption': 10.209, 'std_consumption': 6.649},
                11: {'mean_consumption': 12.044, 'std_consumption': 8.759},
                12: {'mean_consumption': 13.459, 'std_consumption': 10.07}},
    }
    }


    all_results = []

    for simulation in range(num_simulations):
        env = simpy.Environment()
        households = generate_random_households(env, num_households, epc_ratings, imd_quintiles, monthly_parameters)
        env.run(until=num_months * 30)


        simulation_results = []
        for i, household in enumerate(households, 1):
            household_data = {
                'Household': i,
                'EPC Rating': household.epc_rating,
                'IMD Quintile': household.imd_quintile,
            }


            for month, consumption in enumerate(household.monthly_consumptions, 1):
                household_data[f'Month {month}'] = consumption

            household_data['Total Annual Energy Consumption'] = household.total_consumption
            simulation_results.append(household_data)

        all_results.extend(simulation_results)


    csv_filename = "energy_consumption_results.csv"
    with open(csv_filename, mode='w', newline='') as file:
        fieldnames = ['Household', 'EPC Rating', 'IMD Quintile'] + [f'Month {m}' for m in range(1, num_months+1)] + ['Total Annual Energy Consumption']
        writer = csv.DictWriter(file, fieldnames=fieldnames)
        writer.writeheader()
        for result in all_results:
            writer.writerow(result)

    print("Simulation completed successfully. Results saved to:", csv_filename)
